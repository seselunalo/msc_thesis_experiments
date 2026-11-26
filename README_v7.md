# novel_continual_learning v7: running on KENET Chui

v7 is v6 made to run on a SLURM cluster, in particular KENET's **Chui** GPU
cluster. The models, training, metrics and defaults are the same as v6, so
v7 results are directly comparable with v6 results. What changed is where
files go, how progress is saved, and the job scripts (see "What changed from
v6" at the end). The v6 zip, README and notebook are unchanged; keep using
them for Colab if you like.

Anything marked **TODO(Chui)** below and in `slurm/` could not be confirmed
from KENET's public pages. Check it once after you log in (the commands to do
that are given).

## What is known about Chui

From the KENET HPC wiki (`training.kenet.or.ke/index.php/HPC_Usage`,
checked 16 Nov 2026) and the CHUI portal (`ondemand.vlab.ac.ke`):

| Item | Value | Source |
|---|---|---|
| Access | Register at `events.kenet.or.ke/e/gpu-reservation` with your institutional email; KENET sends username, password, hostname, port and **OpenVPN** instructions | wiki "Requesting For Access", "Remote Login" |
| SSH | `ssh -p PORT username@HOSTNAME`, after connecting the VPN | wiki |
| Partitions | `gpu1` (GPU jobs, 24 h max), `normal` (CPU only, 24 h max), `test` (1 min, testing) | wiki "GPU Based Resources With Slurm" |
| GPU request | `--partition=gpu1 --gres=gpu:1` | wiki |
| QOS | `gpu_only` (at most 2 GPUs), `cpu_only`, `debug`, `normal` | wiki "Slurm" |
| Modules | Lmod; `module load applications/gpu/python/conda-25.1.1-python-3.9.21` gives conda with a shared `python-3.9.21` env | wiki "PyTorch and Tensorflow" |
| Scratch | `~/localscratch` ("your scratch directory") | wiki |
| Interactive GPU shell | `srun --time=00:30:00 --gres=gpu:1 --partition=gpu1 --pty /bin/bash -i` | wiki |
| Web portal | Open OnDemand at `ondemand.vlab.ac.ke` (JupyterLab, VS Code, TensorBoard) | portal |
| Help | `support@kenet.or.ke` | portal |

**Not published (TODO(Chui))**: the GPU model and memory on `gpu1`, cores and
RAM per GPU, whether `--mem` is accepted, storage quotas and purge rules for
`~/localscratch`, whether compute nodes have internet, and whether your
account must name a QOS or account. The scripts assume no internet on compute
nodes (the model is downloaded beforehand), which works either way.

**The 24-hour limit** is the main design constraint. The full experiment (5
seeds x 2 datasets x 7 models = 70 runs) takes far longer than 24 h on one
GPU, so v7 splits it into a job array: by default one task per (seed, model
group), 10 tasks, each saving every finished run immediately.

## Files in v7

| File | What it does |
|---|---|
| `run_experiment.py` | The runner, as in v6, plus `--storage-root`, `--prepare-data` and `--aggregate-only` |
| `download_model.py` | Downloads Qwen2.5-1.5B into the Hugging Face cache for offline jobs |
| `slurm/chui_env.sh` | Shared settings: paths, module name, env location. **Edit this one file** |
| `slurm/setup_env.sh` | One-time setup on the login node: env, model, Sheng export, synthetic data |
| `slurm/quick_test.sbatch` | 2-hour plumbing check on one GPU (`--quick`) |
| `slurm/run_seeds.sbatch` | One array task = one seed x one model group |
| `slurm/submit_all.sh` | Submits the array plus a summary job that runs after it |
| `slurm/aggregate.sbatch` | Mean ± std over seeds from all finished runs (CPU job) |
| `run_v7_chui.ipynb` | The same workflow from JupyterLab on Chui's Open OnDemand portal (see "Running from Jupyter") |

## Step by step

### 1. Get access and log in

1. Register (link above) and wait for KENET's email with your credentials and
   VPN profile.
2. Connect the OpenVPN profile on your laptop.
3. Log in from PowerShell or a terminal:

   ```bash
   ssh -p PORT username@HOSTNAME
   ```

   Optional, so you can type `ssh chui` and `scp ... chui:`: add this to
   `C:\Users\Fred\.ssh\config` (Windows) or `~/.ssh/config`:

   ```
   Host chui
       HostName HOSTNAME
       Port PORT
       User username
   ```

4. Look around once and fill in the TODOs:

   ```bash
   sinfo                                   # partitions and nodes
   scontrol show partition gpu1            # time limit, default memory
   scontrol show node <a gpu1 node>        # CPUs, RealMemory, Gres (GPU type)
   sacctmgr show assoc user=$USER format=account,qos%40   # QOS / account you may use
   module avail                            # module names
   df -h ~ ~/localscratch                  # space
   ```

### 2. Upload the code and data

From your laptop (PowerShell works; `scp -P` takes a capital P):

```bash
scp -P PORT novel_continual_learning_v7.zip sheng_clean.csv username@HOSTNAME:~/
```

Then on Chui:

```bash
mkdir -p ~/novel_continual_learning_v7 ~/localscratch/thesis_v7
unzip -o ~/novel_continual_learning_v7.zip -d ~/novel_continual_learning_v7
mv ~/sheng_clean.csv ~/localscratch/thesis_v7/
cd ~/novel_continual_learning_v7
```

If you keep the code elsewhere or want results somewhere other than
`~/localscratch/thesis_v7`, change `CODE_DIR` / `EXPERIMENT_ROOT` in
`slurm/chui_env.sh` (or `export` them before each command).

### 3. One-time setup (login node, about 15 minutes)

```bash
bash slurm/setup_env.sh
```

This creates a Python 3.11 conda env in `~/envs/ncl_v7`, installs
`requirements.txt`, removes `torchao`, downloads Qwen2.5-1.5B (~3 GB) to
`~/localscratch/thesis_v7/hf_cache`, exports the 5 Sheng eras, generates the
20-era synthetic stream, and writes the exact package versions to
`~/localscratch/thesis_v7/environment.txt` (quote these in the thesis).

If `conda create` cannot reach its channels, the script falls back to a venv
on the module's Python 3.9, which the code also supports. If the login node has
no internet at all, ask KENET support how to install pip packages, or build
the env on an OnDemand / interactive session that has internet.

If something after the env step fails (for example `sheng_clean.csv` not
found), fix it and run `bash slurm/setup_env.sh --skip-env`.

### 4. Quick test on a GPU (about 10-30 minutes)

```bash
sbatch slurm/quick_test.sbatch
squeue -u $USER
tail -f logs/ncl-quick-<jobid>.out
```

It prints the GPU name and memory and checks that PyTorch can see the GPU,
then runs all seven models on 5 x 200 synthetic and 50 Sheng posts per era.
It uses its own folder (`$EXPERIMENT_ROOT/quick_test`), so it never touches
the full data. Note the GPU memory: on 30 GB or more, Naive FT and Periodic
fine-tune all weights (`FULL_FT_MODE = "auto"`); below that they use LoRA.
Say which in the thesis, or force one with `EXTRA_ARGS="--ft-mode full"`.

If the job is rejected, the usual causes are: `--mem` not allowed (delete the
`#SBATCH --mem` line), a required QOS (uncomment `#SBATCH --qos=gpu_only`), or
an account (uncomment `--account`).

### 5. Submit the full experiment

```bash
bash slurm/submit_all.sh
```

This submits 10 array tasks (seeds 42-46 x {core, variants}), at most 2
running at once (the `gpu_only` QOS allows 2 GPUs), and a summary job that
starts when they all end. Each task runs one seed on both datasets:

| Group | Models | Rough cost |
|---|---|---|
| `core` | Static, Naive FT, Periodic, Novel | your v5/v6 single-seed time |
| `variants` | DoRA, LoRA+, O-LoRA | about 3.6-4.1 x Naive FT's time |

Other ways to split or narrow it:

```bash
SEEDS="42 43 44" bash slurm/submit_all.sh                       # 3 seeds
MODEL_GROUPS="all" bash slurm/submit_all.sh                      # one task per seed
MODEL_GROUPS="static naive periodic novel dora loraplus olora" bash slurm/submit_all.sh   # one task per model (35 tasks)
DATASETS=sheng MODEL_GROUPS=variants bash slurm/submit_all.sh    # variants on Sheng only
MAX_PARALLEL=1 bash slurm/submit_all.sh                          # one GPU at a time
```

If a task hits 24 h, split finer (one task per model) and submit again.

### 6. Monitor

```bash
squeue -u $USER                                   # pending / running
sacct -j <arrayid> --format=JobID,State,Elapsed,MaxRSS,NodeList
tail -f logs/ncl-v7-<arrayid>_<task>.out          # live output of one task
ls ~/localscratch/thesis_v7/novel_continual_learning/checkpoints/v7_*/seed*/*/   # finished runs
scancel <jobid>                                   # stop a job (finished runs are kept)
```

### 7. Continue after a time-out or failure

Run exactly the same `submit_all.sh` command again. Every task runs with
`--resume`, so finished (seed, dataset, model) runs are skipped and only the
interrupted or failed ones run. Do not run two copies of the same task at the
same time; they would train the same runs twice (harmless, but wasteful).

### 8. Results

The summary job writes, under
`~/localscratch/thesis_v7/novel_continual_learning/experiments/`:

| File | Contents |
|---|---|
| `aggregate_<id>_summary.md` / `.csv` / `.json` | Mean ± std over seeds per dataset and model: AA, AF, FWT, BWT, Macro-F1, time, CPU, RAM, GPU memory |
| `aggregate_<id>.json` | Every run's metrics and accuracy matrices, plus `failed` = runs not finished yet |
| `aggregate_<id>_seed<N>.json` | One seed |
| `results_<date>_job<array>_<task>*.json` | The same, for each array task's own seed and models |
| `experiment_<id>.log` | One log per task |

To redo the summary at any time (for example with a different seed list), on
the login node:

```bash
source slurm/chui_env.sh && load_python_env
python run_experiment.py --aggregate-only --seeds 42 43 44 45 46
```

Copy everything back to your laptop (from your laptop):

```bash
scp -P PORT -r username@HOSTNAME:~/localscratch/thesis_v7/novel_continual_learning/experiments ./chui_results
scp -P PORT username@HOSTNAME:~/localscratch/thesis_v7/environment.txt ./chui_results/
```

or `rsync -av -e "ssh -p PORT" ...` if rsync is available (it is not on
plain Windows; use WSL or scp there).

### Plotting

The accuracy matrix of any run is in the results JSON:

```python
import json, numpy as np, matplotlib.pyplot as plt
res = json.load(open("chui_results/aggregate_<id>.json"))
R = np.array(res["results_by_seed"]["42"]["sheng"]["Novel-Sheng"]["acc_matrix"], dtype=float)
plt.imshow(R, vmin=0, vmax=1, cmap="viridis"); plt.colorbar(label="accuracy")
plt.xlabel("test era j"); plt.ylabel("eras trained (r)"); plt.show()
```

## Running from Jupyter (Open OnDemand)

`run_v7_chui.ipynb` does everything above from JupyterLab in a browser, with
cluster paths only (no Google Drive):

1. Connect the VPN, open `ondemand.vlab.ac.ke`, start **Interactive Apps →
   JupyterLab** on `gpu1` with 1 GPU and up to 24 hours. TODO(Chui): the
   form's exact fields are not published.
2. Upload `novel_continual_learning_v7.zip`, `sheng_clean.csv` and
   `run_v7_chui.ipynb` to your home folder with the JupyterLab file browser.
3. Run the notebook from the top. Section 1 finds the uploads and copies the
   CSV to `~/localscratch/thesis_v7/sheng_clean.csv`; every later step
   passes that path explicitly, so the Sheng export never looks for a Drive
   or Windows path, and a missing CSV is reported before anything runs.
4. Chui's compute nodes, including JupyterLab sessions, have **no
   internet**, so the one-time setup (`slurm/setup_env.sh`: env, model
   download, Sheng export, synthetic data) runs on the **login node**: open
   **Clusters → Shell Access** in OnDemand and run
   `cd ~/novel_continual_learning_v7 && bash slurm/setup_env.sh`. Section 4
   of the notebook then just checks that it finished. Run from a node without
   internet, the script stops at once with these instructions.
5. Section 6 is the quick check. For the full run, section **7b** submits
   the batch array from the notebook (recommended: it runs whether or not the
   session is open, then end the session to free its GPU). Section **7a**
   runs it inside the session in the background instead; a JupyterLab
   session stops at its time limit, so start a new one and run 7a again
   (`--resume` skips finished runs).
6. Section 8 shows the mean ± std tables, section 9 plots an accuracy matrix,
   section 10 zips the results for download.

The notebook and the batch scripts share the same folders and finished runs,
so you can mix them: for example submit with 7b and read results in a short
CPU-only Jupyter session. Don't run 7a and 7b on the same seeds at once.

## Running v7 elsewhere

Everything works without SLURM too:

```bash
pip install -r requirements.txt && pip uninstall -y torchao
python -m data.export_sheng --csv sheng_clean.csv --storage-root ./experiment_data
python run_experiment.py --storage-root ./experiment_data --quick
python run_experiment.py --storage-root ./experiment_data --seeds 42 43 44 45 46 --resume
```

On Colab, use `--storage-root /content/drive/MyDrive/thesis_experiments`
(v7 no longer finds Google Drive by itself). Without `--storage-root` or
`EXPERIMENT_ROOT`, files go to `./experiment_data`.

## What changed from v6

| Area | v6 | v7 |
|---|---|---|
| Storage | Detected Lightning AI, Colab and Google Drive; otherwise `./experiment_data` | `--storage-root` > `EXPERIMENT_ROOT` > `./experiment_data`. No platform detection |
| Progress / resume | One `checkpoint_v6.json` for all runs. Two jobs at once would overwrite each other's progress | One file per finished (seed, dataset, model) run in `checkpoints/v7_<settings hash>/`, written atomically. Any number of array tasks can run at once. Runs with other settings (`--quick`, `--model`, `--ft-mode`, `--data-seed`) go to another folder and never mix |
| Data preparation | Generated on first use | Same outside SLURM. `--prepare-data` generates it and exits; inside an array task the runner refuses to generate (parallel tasks would race) and tells you to run `--prepare-data` |
| Summary | Only for the seeds of the run that just finished | Also `--aggregate-only`: the summary from every finished run on disk, filtered by `--seeds`, `--models`, `--datasets`. Missing runs are listed |
| Missing Sheng export | Sheng silently skipped | Error with the export command |
| Experiment ID | Timestamp | Timestamp plus `_job<array>_<task>` under SLURM; logs and results record job, node, partition and torch version |
| Model download | At first run (needs internet) | `download_model.py` beforehand; jobs run with `HF_HUB_OFFLINE=1` |
| `export_sheng` default CSV | `C:/Users/Fred/Desktop/claude/sheng_clean.csv` | `sheng_clean.csv` in the storage root |
| Requirements | | Adds `huggingface_hub` (already installed with transformers) |

The models, training loop, metrics, defaults and the results JSON layout are
unchanged from v6. See `README_v6.md` for the methods, the LoRA-variant
baselines, the resource columns and the multi-seed design.

**Notebook tested (23 Nov 2026)** the same way: sections 1-9 run in a
fake home folder with the tiny model (setup script and 7b skipped, since
there is no conda module or SLURM here).

**Tested (16 Nov 2026)** on CPU with Python 3.13, torch 2.15, transformers
5.23 and peft 0.21, using a tiny randomly initialised Qwen2 model (the real
model could not be downloaded in the test environment): data preparation,
three simulated array tasks running at the same time, resume, the
refuse-to-generate check, and `--aggregate-only`. Not tested on Chui itself
or on a GPU.
