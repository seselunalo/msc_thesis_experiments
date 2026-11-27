# Data work record: Sheng sentiment corpus (MSc thesis, JKUAT)

*Adaptive NLP for Rapidly Evolving Languages: A Continual Learning Approach with Application to Sheng* (proposal V20260706-2.1).
Record of the data work done in this chat, written 2026-11-25. All data files are in the chat's outputs folder.

## 0. How to read this record

Every figure carries one of three confidence tags, because part of this chat's history was lost when the working environment was reset.

| Tag | Meaning |
|---|---|
| **[V]** | Verified: recomputed from the files on 2026-11-25, by the code in the appendix. |
| **[S]** | Reported earlier in the chat (earlier messages, `cleaning_report.md`, or the summary written when the chat's context was compacted). The underlying files or scripts no longer exist, so I could not re-check it. |
| **[?]** | Not known or not recorded anywhere I can reach. Left open on purpose. Do not fill these with estimates. |

What was lost, and what it means:

* The original pipeline scripts (`build_master`, `clean_master`, `compute_exclusions`, `finalize`, `clean_pii_ascii`, `emoji_sentiment`, the id registry and the original human-label file) were wiped by sandbox resets. **Their source code cannot be reproduced.** Section 8 lists them as "not recoverable" and gives the regular expressions and the descriptions that survive. It does not invent code.
* The saved transcript on disk starts at the compaction on 2026-10-08, so everything before that date is known only from the summary and from the files themselves.
* Counts in sections 2 to 6 come from the output files, which survived. Where a step's count cannot be derived from the files, it is tagged [S] or [?].

## 1. Data sources

### 1.1 What the corpus is made of [V for counts, S for descriptions]

The corpus is 25,886 records from 24 source groups across Twitter/X, Facebook, TikTok, Instagram and curated or pasted Sheng collections. Per-source record counts are in section 2. All of it reached this chat as **files you uploaded or text you pasted**; nothing was scraped or fetched by me. The one use of the web was to look up Sheng vocabulary for the lexicon (section 3.8), which is not corpus text.

| Source group (as named in the data) | Platform | What it is | How it was collected | Date evidence |
|---|---|---|---|---|
| `twitter_scrape` | Twitter/X | 49 export files, `twitter_data_batch_*` and `twitter_data_2026-09-0*` (schema: date, author, post, empty `sentiment`) | Scraper exports, uploaded by you. Tool and method [?]. | Dates in the rows; file names carry 2026-09-08 and 2026-09-09 |
| `zeeschuimer_twitter` | Twitter/X | Zeeschuimer browser-extension export | Browser capture while browsing, by you. [S] from the tool's name and schema | File name `2026-08-23` |
| `zeeschuimer_tiktok` | TikTok | Zeeschuimer export (36 columns incl. `body`, `hashtags`, play counts) | Browser capture [S] | File name `2026-08-31` |
| `zeeschuimer_instagram` | Instagram | Zeeschuimer export; its CSV quoting was corrupted, so only 11 captions were recovered [S] | Browser capture [S] | File name `2026-08-31` |
| `twitter_sheng_flag` | Twitter/X | `twitter_data_with_sheng_flag.csv` (columns `post_date, author, post_text, has_sheng, sheng_terms_str`) | Prior pipeline output with a Sheng flag, uploaded by you; origin [?] | Dates in the rows |
| `facebook_with_dates`, `facebook_manual`, `facebook_generic` | Facebook | `facebook_posts_with_dates*.csv`, `facebook_posts_manual*.csv` and 21 `facebook_data_<mm-yy>_batch_<n>_2026-09-09T*.csv` files (columns `Date, Author, Post/Comment, Parent Post, Type, Hashtags`) | Scraper and manual copy exports, uploaded by you. Tool and method [?] | File names carry `2026-09-09`; row dates vary |
| `Matwana_2020`, `Matwana_2021`, `Matwana_2022` | Facebook | Posts and comments from the Matwana source, one group per year | Pasted or uploaded by you [S]; method [?] | Year in the group name |
| `Kartelo`, `bongasheng` | Facebook | Comment sets from two Facebook pages | Method [?] | Row dates |
| `sheng_social_media_dataset`, `sheng_social_media_dataset_2` | Unspecified social | Curated Sheng posts with a `sentiments(PG or None PG)` column (a content rating, not sentiment) | Uploaded by you; original compiler [?] | Row dates |
| `sheng_summary_composite`, `sheng_summary_no_urls` | Unspecified social | Summary tables with columns for Sheng used and meaning | Uploaded by you; compiler [?] | Row dates |
| `sheng_news_posts` | News / Facebook | News-page posts, columns include `Sheng Used`, `Meaning`, `Label` | Uploaded by you | Row dates (e.g. `4-Jul-24`) |
| `sheng_mtaa_twitter` | Twitter/X | Posts from a "Sheng Mtaa on X" slang-of-the-day account; almost entirely glossary-annotated [S] | Pasted by you [S] | Row dates (month-first with time) |
| `sheng_manual_batch2_*` (`sheng_news`, `Ghetto_Radio_Sheng_News`, `Lughayangu`, `shengtezzo`, `Facebook_Matwana`) | Unspecified social | A second manual paste of five sources (497 rows in the paste [S]) | Pasted by you [S] | Row dates |

Dates here mean what the files themselves say (file-name timestamps or row dates). **I have no record of the dates you actually ran the collections**, only of when files reached the chat. [?]

### 1.2 Permissions and ethics

What the proposal commits to (Section 3.6.2 and 3.8 of `SESERA - THESIS PROPOSAL V20260706-2.1`) [V, read from the docx]:

* Collect public posts from Twitter/X, Facebook, Reddit and forums through API access and ethical web scraping.
* Restrict collection to public-facing content where users have no reasonable expectation of privacy.
* Remove all personally identifiable information using rule-based systems and a fine-tuned NER model.
* Submit the collection and handling protocol to the **JKUAT Ethics Review Committee for formal approval before any data collection begins**.
* Store data encrypted with access restricted to the research team; scripts identify themselves with a user agent and researcher contact.
* Adhere to platform terms of service (the proposal names the Twitter Developer Agreement and the Reddit API terms).

What this chat shows against those commitments:

* **Ethics approval: [?]** No approval letter, reference number or date appears anywhere in this chat. I cannot say whether it was obtained, or whether it was obtained before the files dated 2026-08 and 2026-09 were collected. Please add this from your own records.
* **Platform terms: not verified.** I do not know how the Twitter/X, Facebook and TikTok exports were collected. In this chat I advised that scraping Facebook, X and TikTok can breach platform terms and may engage Kenya's Data Protection Act 2019, and listed access routes that avoid that (Meta Content Library, the YouTube Researcher Program where eligible, consent-based collection, written agreements with content owners). That was general guidance and not legal advice. Whether the corpus complies is an open question for you and your supervisors.
* **Reddit, forums and blogs:** named in the proposal but **absent from the corpus**. No Reddit data was provided.
* **Anonymisation actually done:** section 3.7. It is rule-based only. **No NER model was run**, and names inside post text were flagged but not removed.
* **Storage and access:** nothing in this chat shows encryption or access control. The output CSVs sit unencrypted in the chat's outputs folder. [V] The master file still contains an `author` column (18,621 non-empty values), which is identifying; the two delivered datasets leave it out.

## 2. Raw data

### 2.1 Records per source before cleaning [V]

"Registry total" is every record that ever received a stable row id (`R000001` onward) = kept rows + rows in `excluded_rows_log.csv`. The registry is **additive**: when a later upload overwrote an earlier file of the same name, the earlier rows were kept, so these totals include versions of files that no longer exist as uploads. Because of this, the registry total cannot be reproduced from the upload folder alone.

| source_group | platform | n_files | registry_total | excluded | kept |
|---|---|---|---|---|---|
| twitter_scrape | twitter | 49 | 15,594 | 1,065 | 14,529 |
| sheng_social_media_dataset | unspecified_social | 1 | 2,310 | 268 | 2,042 |
| sheng_social_media_dataset_2 | unspecified_social | 1 | 2,308 | 1,785 | 523 |
| facebook_manual | facebook | 2 | 885 | 194 | 691 |
| zeeschuimer_twitter | twitter | 1 | 855 | 96 | 759 |
| twitter_sheng_flag | twitter | 1 | 841 | 443 | 398 |
| Matwana_2022 | facebook | 1 | 632 | 38 | 594 |
| facebook_with_dates | facebook | 2 | 578 | 27 | 551 |
| facebook_generic | facebook | 17 | 301 | 94 | 207 |
| sheng_mtaa_twitter | twitter | 1 | 273 | 155 | 118 |
| sheng_manual_batch2_Facebook_Matwana | unspecified_social | 1 | 204 | 198 | 6 |
| Kartelo | facebook | 1 | 204 | 20 | 184 |
| zeeschuimer_tiktok | tiktok | 1 | 137 | 1 | 136 |
| sheng_summary_no_urls | unspecified_social | 1 | 134 | 57 | 77 |
| sheng_summary_composite | unspecified_social | 1 | 119 | 57 | 62 |
| sheng_manual_batch2_shengtezzo | unspecified_social | 1 | 113 | 100 | 13 |
| Matwana_2020 | facebook | 1 | 90 | 1 | 89 |
| sheng_manual_batch2_sheng_news | unspecified_social | 1 | 88 | 11 | 77 |
| sheng_news_posts | news_facebook | 1 | 86 | 27 | 59 |
| sheng_manual_batch2_Ghetto_Radio_Sheng_News | unspecified_social | 1 | 56 | 1 | 55 |
| sheng_manual_batch2_Lughayangu | unspecified_social | 1 | 36 | 0 | 36 |
| bongasheng | facebook | 1 | 19 | 2 | 17 |
| Matwana_2021 | facebook | 1 | 12 | 0 | 12 |
| zeeschuimer_instagram | instagram | 1 | 11 | 1 | 10 |
| **TOTAL** |  | 90 | 25,886 | 4,641 | 21,245 |

`n_files` counts distinct `source_file` names in the registry (including earlier versions of overwritten files), so its total (90) differs from the number of files currently in the upload folder.

By platform:

| platform | kept | excluded | registry_total |
|---|---|---|---|
| twitter | 15,804 | 1,759 | 17,563 |
| unspecified_social | 2,891 | 2,477 | 5,368 |
| facebook | 2,345 | 376 | 2,721 |
| tiktok | 136 | 1 | 137 |
| news_facebook | 59 | 27 | 86 |
| instagram | 10 | 1 | 11 |

The registry total is **25,886** records, of which **21,245** were kept and **4,641** excluded. [V]

Earlier milestones, reported at the time [S]:

* 84 raw CSV files, 7 distinct schemas, **20,034 raw rows ingested** at the first cleaning (`cleaning_report.md`). The report does not say whether this figure is before or after the four whole-file drops in 3.1. [?]
* 5,082 rows were added in the 2026-09-28 update (nine new Twitter batch files plus changed ones), taking the registry to 25,886. The rows are in `records_added_today_2026-09-28.csv`. [S; the file is present with 5,082 rows [V]]
* About 770 further rows came from pasted sources (273 `sheng_mtaa_twitter` and 497 across five sources). [S]

### 2.2 Formats of the raw upload files [V]

The upload folder in this session holds 87 CSV files (plus the proposal `.docx` and the labelled `.xlsx`). A default CSV reader parses them into 23,301 rows in total. This is **not** the registry total, because several files are duplicates or overwritten versions, and two files (a Zeeschuimer Twitter export and `sheng_summary_composite.csv`) have encoding or quoting faults that the default reader cannot parse.

| files | rows (naive parse) | columns | example file |
|---|---|---|---|
| 40 | 11,493 | `Post/comment Date; author; Post; sentiment` | twitter_data_2026-09-08.csv |
| 16 | 243 | `Date; Author; Post/Comment; Parent Post; Type; Hashtags` | facebook_data_02-05_batch_9_2026-09-09T07-53-04.csv |
| 9 | 1,402 | `Post/comment Date;author;Post;sentiment` | twitter_data_batch_1__3_.csv |
| 5 | 54 | `Date;Author;Post/Comment;Parent Post;Type;Hashtags` | facebook_data_02-26_batch_7_2026-09-09T08-07-53.csv |
| 4 | 976 | `Author; Display Date; ISO Timestamp; Text` | facebook_posts_with_dates.csv |
| 3 | 6,928 | `recordID; Post/commentDate; post; shengused; sentiments(PG or None PG)` | sheng_social_media_dataset.csv |
| 2 | 885 | `Count; Author; Display Date; ISO Timestamp; Text` | facebook_posts_manual.csv |
| 2 | 0 | `(could not be parsed by the default CSV reader)` | sheng_summary_composite.csv |
| 2 | 936 | `post_date; author; post_text; has_sheng; sheng_terms_str` | twitter_data_sheng_only.csv |
| 1 | 86 | `Date; PostContent; Hashtags; Sheng Used'; Meaning ; Label` | sheng_news_posts.csv |
| 1 | 134 | `timestamp; content; sheng used; meaning; author` | sheng_summary_no_urls.csv |
| 1 | 22 | `collected_from_url;collected_from_view;partial_item;id;timestamp;thread_id;parent_id;url;body;author_id;author;author_fullname;verified;a...` | zeeschuimer-export-instagram_com-2026-08-31T150951.csv |
| 1 | 142 | `collected_from_url; id; thread_id; author; author_full; author_followers; author_likes; author_videos; author_avatar; body; stickers; tim...` | zeeschuimer-export-tiktok_com-2026-08-31T150953.csv |

Delimiters differ: some files are comma-separated and some semicolon-separated, which is why the same schema appears twice with different column strings. The Twitter files have a `sentiment` column, and every value in it is empty. [S, `cleaning_report.md`: "0 of 10,691 rows filled"]

## 3. Cleaning and preprocessing

### 3.1 Steps in the order they were done [S unless marked]

1. **Parse seven schemas into one** (`row_id, source_group, platform, source_file, author, date_raw, text, ...`). [S]
2. **Drop four redundant files in full** because a larger file already contained them: `facebook_posts_with_dates.csv` (347 rows, all in `__1_`), `facebook_posts_with_dates__2_.csv` (51 rows, all in `__3_`), `sheng_social_media_dataset__1_.csv` (2,310 rows, byte-identical to the original), `twitter_data_sheng_only.csv` (95 rows, all in `twitter_data_with_sheng_flag.csv`). That is 2,803 rows. [S]
3. **Assign permanent row ids** through a registry so labels keyed to an id never move when rows are added. [S] The registry file is lost. [V]
4. **Row-level exclusion**: flag a row for one or more reasons, remove it from the master and write it to `excluded_rows_log.csv` (below).
5. **Stylised-font normalisation**: mathematical-alphabet characters mapped back to plain Latin in `text_display`; the raw string is kept in `text`. Reported as 88 rows at the first cleaning [S]; 8 rows carry the flag in the current master [V]. I did not record why the count dropped.
6. **URL stripping** from `text_display` (raw kept in `text`). [S]
7. **Plain-text and emoji filter** to make `text_plain_emoji`: keep letters, digits, punctuation, whitespace and emoji; drop stray symbols, invisible marks and mojibake bytes. No row became empty or punctuation-only. [V counts below]
8. **Emoji heuristic** to make `emoji_sentiment` (section 4.7).
9. **PII and ASCII clean** to make `text_clean` (3.7).
10. **Sheng lexicon flag** to make `contains_sheng` and `matched_sheng_terms` (3.8). This is re-run every time the lexicon changes.

The order of the exclusion rules inside step 4 is **not recorded** [?]. Because one row can carry several reasons, a sequential "records removed per step" funnel would be invented. What I can give exactly is the count per reason and the overlaps.

### 3.2 Row-level exclusions, per logged reason combination [V]

| exclusion_reasons (as logged) | rows |
|---|---|
| duplicate | 4,197 |
| punctuation_only | 168 |
| truncated_see_more | 110 |
| punctuation_only;duplicate | 86 |
| spam_or_promo | 42 |
| duplicate;truncated_see_more | 15 |
| scraper_artifact;duplicate | 10 |
| scraper_artifact;punctuation_only;duplicate | 10 |
| scraper_artifact | 1 |
| scraper_artifact;punctuation_only | 1 |
| duplicate;spam_or_promo | 1 |

Per reason, counting a row once in each reason it carries (a row with two reasons appears twice, so these sum to more than 4,641):

| reason | rows |
|---|---|
| duplicate (any combination) | 4,319 |
| punctuation_only (any) | 265 |
| truncated_see_more (any) | 125 |
| spam_or_promo (any) | 43 |
| scraper_artifact (any) | 22 |

These match the totals reported in `cleaning_report.md` (4,319 / 265 / 125 / 43 / 22). [V]

* **Duplicate**: second and later occurrence of identical text after Unicode, whitespace and case normalisation. The first occurrence is kept and its `duplicate_count` records how many copies existed. The dedup was fixed so that a labeled row is never dropped in favour of an earlier unlabeled copy. [S] 2,933 kept rows have `duplicate_count` above 1. [V]
* **Truncated**: text ending in a client-side "... See more". [S]
* **Punctuation or digits only**: for example `,,,,` or `2100`. Emoji-only comments were deliberately kept. [S]
* **Spam or promo**: bare links or "subscribe / click here" posts. 1,502 other rows containing a URL were kept. [S]
* **Scraper artifact**: the literal template rows "Physics will prove you wrong" and ",,,," that appeared as the first rows of 9 or more unrelated files. [S]

Result: 25,886 registry rows - 4,641 excluded = **21,245 kept** in `cleaned_population.csv`. [V]

### 3.3 Text-level steps on the 21,245 kept rows [V]

| check | rows |
|---|---|
| text != text_display (URL stripped / stylised fonts mapped / whitespace) | 5,538 |
| text_display != text_plain_emoji (stray symbols, invisible marks, mojibake dropped) | 492 |
| rows emptied or punctuation-only by the plain+emoji filter | 0 |
| rows with an @mention in text_plain_emoji | 1,768 |
| rows with a #hashtag in text_plain_emoji | 2,782 |
| rows with an e-mail address in text_plain_emoji | 8 |
| rows with a phone-like number (7-12 digits) in text_plain_emoji | 393 |
| rows with at least one non-ASCII character (incl. emoji) in text_plain_emoji | 7,688 |
| rows where text_clean != text_plain_emoji | 9,826 |
| rows empty after PII + ASCII clean (kept in master, left out of every export) | 85 |
| rows flagged possible_leading_name (flag only, NOT removed) | 1,398 |
| residual check: rows in text_clean with non-ASCII / @ / # / URL | 0 / 0 / 0 / 0 |

### 3.4 Normalisation [S]

Stylised fonts mapped to Latin, URLs removed from the display text, stray symbols and mojibake removed. The proposal's normalisation items "colloquial spellings, elongated words, emoji conversion" were, as far as I can tell, **not** done (no such step was run in this chat): spelling variants and elongations are left as written, and emoji are deleted from `text_clean` and are not converted to text.

### 3.5 Accent handling [V]

`text_clean` is ASCII only (0 non-ASCII characters remain). Accented letters were **deleted**, not transliterated, so a word such as one containing `ê` loses that letter. I offered transliteration (ê to e) and the offer was never answered.

### 3.6 Language detection [V]

**None was run.** There is no language-ID step. "Sheng" is decided only by the lexicon match in 3.8, so a row flagged as Sheng is one that contains a listed term and is not verified as Sheng by any language-ID model or annotator.

### 3.7 Anonymisation (PII) [V for counts, S for code]

Removed from `text_clean`, in this order (regular expressions recovered from the saved summary; the script itself is lost):

1. URLs (`https?://\S+|www\.\S+`).
2. E-mail addresses.
3. @mentions (`@\w+`).
4. #hashtags (`#\w+`).
5. Phone-like numbers: Kenyan mobile pattern `(?:\+?254|0)?7\d{8}` or any 7 to 12 digit run.
6. All non-ASCII characters (this deletes every emoji), then whitespace collapsed.

Row counts for each of these are in the table in 3.3. Limits:

* **Names written in plain text are not removed.** The proposal promises a fine-tuned NER model; none was built or run. Rows where the text starts with something name-like (two to four capitalised words) are only **flagged** (`possible_leading_name`, 1,398 rows), and the examples in section 6 use only unflagged rows.
* The phone pattern also removes any long digit string (prices, ids). Numbers such as "Ksh 1500" are kept; seven-digit-plus numbers are not.
* The master still holds the author handle or name in `author`. Anonymisation is applied to the text columns and to the two delivered datasets, which exclude `author`. The master file should be treated as sensitive.
* The `author_hash` approach I recommended in the schema advice was **not implemented**.

### 3.8 Sheng lexicon and the Sheng flag

The Sheng flag is a **whole-word, case-insensitive regular-expression match** of lexicon terms against `text_plain_emoji`, longest term first. The word `form` does not match when followed by `1`-`4` or `one`-`four` (to avoid "form 3", a school year). Code is in the appendix (`filter_sheng_only.py`).

Lexicon size now: **889 terms** [V]. Where the terms came from [V counts, S for attribution]:

| meaning provenance | terms |
|---|---|
| other (glossary-derived or earlier batches, provenance not itemised) | 410 |
| user-confirmed meaning | 262 |
| sheng.co.ke Kamusi (T-section), meaning not verified | 109 |
| web-sourced meaning | 53 |
| user-added, no meaning recorded | 51 |
| blank | 4 |
| **TOTAL** | 889 |

* The base lexicon was built from glossary-style rows inside the corpus plus web sweeps (sheng.co.ke, and other sites I searched, whose pages I read as web results), and was extended by your messages in the form `word=meaning`. User-confirmed meanings are marked `(user-confirmed)` in `sheng_words_list.csv`.
* 290 of 889 terms match no row in the corpus. [V]
* **Risky words kept at your instruction** because they also occur as ordinary English or Swahili: `time`, `so`, `k`, `red`, `tano`, `toka`, `kudade`, `dre`, `chacha`, `jake`, `kutu`, `dumbo`, `mongo`, `form`, `sonko`, `chai`, `scene`, `blue`, `bob`. [S] I recommended removing `so`, `k` and `red` from the active filter and you did not answer, so they are still active.
* The most frequent matched terms (rows matched): `so` 529, `time` 499, `mbaya` 235, `dem` 193, `mtaa` 163, `wasee` 163, `msee` 157, `joh` 145, `rada` 130, `doba` 122, `mbogi` 108, `fiti` 106. [V] `so` and `time` are among the top two, which shows how much the flag over-matches ordinary English.
* Result: **5,502 of 21,245 rows (25.9%) are flagged as containing Sheng** [V], held in `cleaned_population_sheng_only.csv` (5,502 rows).

### 3.9 Exports built from the master

* `sheng_labeled_dataset.csv` and `sheng_unlabeled_dataset.csv` were built from the current master (section 5).
* `sentiment_dataset.csv` and `labeling_workbook.xlsx` are **stale** (21,160 rows each, built before the lexicon grew and before the tap-labeling session). Do not use them for counts. Rebuild them from the master if you want them current.

## 4. Labelling

### 4.1 Label set [V]

Three classes are stored: **Positive, Neutral, Negative**. The proposal specifies **binary (positive / negative)** (Section 3.6.2). The mismatch is unresolved: either the proposal needs an amendment to three classes, or Neutral rows are set aside for the binary experiments. In the data, human labels are 793 Positive, 548 Neutral and 767 Negative. [V]

### 4.2 What each label means, and guidelines [?]

* The only written definition anywhere is the codebook sheet in `labeling_workbook.xlsx`: `sentiment_label = positive / negative / neutral`, and `label_source = human = your manual label; emoji_heuristic = inferred from emoji; blank = unlabeled`. [V]
* **No annotation guideline (decision rules for sarcasm, code-switching, quotes, news headlines, questions, and so on) was written or shared in this chat.** The proposal calls for "a standardised annotation guideline"; it does not exist here. [?]
* In the tap-labeling interface the only options were Positive, Neutral, Negative and Skip. A Skip means "do not label" and is not a class. [V]

### 4.3 Who labelled, and how many labellers [?]

* All human labels in this chat were entered or supplied by **you**: a large pasted batch, a labelled spreadsheet you uploaded, and the tap-labeling sessions. I did not label any row and no AI-produced sentiment labels exist in the data. [S, consistent with the label columns [V]]
* **The number of labellers is not recorded.** The files do not say whether the pasted batch and spreadsheet were produced by one person or several, and the proposal calls for "native Sheng speakers" (plural). Fill this in from your own records. [?]
* The tap-labeling answers (106 labels) are in `labeling_session_log.csv` with a UTC timestamp, from the `2026-10-07` to `2026-11-09` sessions. [V]

### 4.4 Provenance of the 2,108 human labels [V]

| Stage | Human labels | Evidence |
|---|---|---|
| Before the tap-labeling sessions | **2,002** | Derived: 2,108 total - 106 tap labels. Matches the 2,002 reported when the pasted batch and the spreadsheet were merged [S], and the 733 + 533 + 736 = 2,002 human rows in the stale `sentiment_dataset.csv` [V]. |
| Tap-labeling sessions | **106** | `labeling_session_log.csv`: 57 Positive, 15 Neutral, 34 Negative; plus 20 skipped |
| **Total** | **2,108** | `existing_label_type == manual_sentiment_label (Positive/Negative/Neutral)` in `cleaned_population.csv` |

How the earlier labels arrived [S]:

* An initial 56 human labels existed on `sheng news` rows (54 survived cleaning).
* A batch of 1,875 hand-labelled rows (676 Negative, 697 Positive, 502 Neutral) was merged by `row_id`. Checks before merging: every id existed, none repeated, none conflicted, and 46 of 47 rows that already carried a label agreed. The one disagreement was `R020386` (earlier Negative, new Positive); the new label was taken.
* A second paste had text corruption (mojibake), and 43 of 354 rows would not match, so you uploaded a spreadsheet and the merge was redone from it by id (reported total 2,002). One conflict was found, `R000067` (Negative, then Positive); I applied the newer value and told you.
* Codes `pg` and `d` in the spreadsheet were **never applied** as sentiment. You have since said that `pg` means the content is not appropriate for children and `none pg` means it is appropriate. That is a content rating, not sentiment.

**Disagreement resolution: there was no multi-annotator disagreement process.** Where a row received two different labels from the same source (the two cases above), the later value replaced the earlier one. [S]

### 4.5 Inter-annotator agreement [V]

**None was measured.** No row was labelled by two annotators in this chat, so there is no Cohen's kappa or any other agreement score. The proposal targets kappa of at least 0.80 and requires it. Section 7 lists this as the biggest methodological gap.

### 4.6 Tap-labeling sample design [S for design, V for counts]

The queue of 2,070 rows was selected from natural (non-glossary), unlabeled, Sheng-flagged rows of 30 to 220 characters and at least 5 words, in the eras 2023-H2 through 2026-H1, de-duplicated on normalised text, and interleaved round-robin across eras with random seed 7. The queue's own contents: 2023-H2 61, 2024-H1 92, 2024-H2 175, 2025-H1 241, 2025-H2 321, 2026-H1 1180. Status now:

| status | rows |
|---|---|
| pending | 1,941 |
| labeled | 106 |
| skipped | 20 |
| shown | 3 |

Because of the round-robin, the 126 logged answers are balanced across eras (21 per era). [V]

### 4.7 Emoji-heuristic labels (not human) [V counts, S for rule]

* The master has 935 rows with an `emoji_sentiment` value: 848 Positive and 87 Negative; it never assigns Neutral.
* 7 of them also have a human label; the human label takes priority, which leaves **928** emoji-only labels.
* **The rule (which emoji count as positive or negative, and how ties are handled) is not recoverable.** The script is lost and I did not record the emoji lists. [?]
* These are weak, unverified labels. They are 90% Positive. Use them for pre-training or analysis if at all, and not as ground truth for evaluation.

## 5. Final dataset

### 5.1 Files produced [V]

| file | rows | columns | purpose |
|---|---|---|---|
| cleaned_population.csv | 21,245 | 25 | Master table: every kept row, all text versions, all label columns, Sheng flag |
| cleaned_population_sheng_only.csv | 5,502 | 24 | Rows of the master whose text matches the Sheng lexicon (current labels) |
| excluded_rows_log.csv | 4,641 | 16 | Every row removed in cleaning, with exclusion_reasons |
| sheng_labeled_dataset.csv | 3,036 | 8 | Delivered: all labeled rows (human + emoji heuristic), cleaned text, Sheng words and meanings |
| sheng_unlabeled_dataset.csv | 4,003 | 6 | Delivered: Sheng rows with no sentiment label of any kind, label column blank |
| sentiment_dataset.csv | 21,160 | 8 | STALE snapshot of the earlier export (built before most of the lexicon growth and tap labeling) |
| labeling_workbook.xlsx | 21,160 | 9 | STALE annotation workbook (Labeling + Codebook sheets); same snapshot as sentiment_dataset.csv |
| sheng_words_list.csv | 889 | 3 | Lexicon with documented meaning and corpus match count |
| label_queue.csv | 2,070 | 4 | Tap-labeling queue (row_id, era, text_clean, status) |
| labeling_session_log.csv | 126 | 4 | Audit log of tap-labeling answers (label or skip, era, UTC time) |
| records_added_today_2026-09-28.csv | 5,082 | 12 | The 5,082 rows added in the 2026-09-28 update (as reported in cleaning_report.md) |
| records_added_today_sheng_only_2026-09-28.csv | 1,291 | 13 | Sheng-matching subset of that update |
| cleaning_report.md |  |  | Earlier cleaning report; figures in it are from an earlier state of the data |

### 5.2 Columns

`sheng_labeled_dataset.csv`: `id`, `date_posted`, `text`, `contains_sheng`, `sheng_words_used`, `sheng_meanings`, `label`, `label_source`.

`sheng_unlabeled_dataset.csv`: `id`, `date_posted`, `text`, `sheng_words_used`, `sheng_meanings`, `label` (blank).

* `id` is the permanent `R######` row id. `date_posted` is `dd/mm/yyyy`, `mm/yyyy` or `yyyy`, converted from about 20 raw formats (rules in `datefmt.py`). `text` is `text_clean` (ASCII, PII removed). `sheng_meanings` lists `term = meaning` only for terms with a recorded meaning (placeholder and "not verified" entries are treated as no meaning).
* In `sheng_labeled_dataset.csv`, `label_source` is `human` or `emoji_heuristic (unverified)`.
* **Dates**: the raw formats mix day-first (`dd/mm/yyyy`, 8,895 rows with a first part above 12 and none with a second part above 12 when this was checked [S]), ISO year-first, month-first with time (`sheng_mtaa_twitter` only), `d-Mon-yy`, month names, `mm/yy`, and bare years. A year-only date (594 rows, almost all `Matwana_2022`) cannot be placed in a half-year. Many Facebook sources are dated 2026 and **may be the scrape or export date rather than the post date**, so 2026 counts may be inflated. `02/05` (9 rows from `facebook_generic`) and `0` (12 rows) cannot be read as a date at all and are left blank, giving 8 blank dates in `sheng_labeled_dataset.csv` and 2 in `sheng_unlabeled_dataset.csv`.
* **Date-converter fix, 2026-11-25.** The first versions of both datasets left 11 `Lughayangu` rows with dates like `January 15, 2023` blank (9 of them labeled rows). I added that format to the converter and rebuilt `sheng_labeled_dataset.csv`; `sheng_unlabeled_dataset.csv` was rebuilt too and did not change. Download the labeled file again if you saved the earlier copy.

### 5.3 Row counts and class counts [V]

`sheng_labeled_dataset.csv` (3,036 rows):

| label_source | Positive | Neutral | Negative | Total |
|---|---|---|---|---|
| emoji_heuristic (unverified) | 842 | 0 | 86 | 928 |
| human | 793 | 548 | 767 | 2,108 |
| **ALL** | 1,635 | 548 | 853 | 3,036 |

* Rows that contain a Sheng term: 1,499; rows that do not: 1,537.
* Human-labeled rows that contain Sheng: 1,235 of 2,108.
* Binary (Positive + Negative, human only): **1,560** rows, which is about 13% of the proposal's ~12,000 labelled-example target.

`sheng_unlabeled_dataset.csv`: **4,003** rows, `label` blank, with every row carrying at least one Sheng term. It excludes every human-labeled row, every emoji-labeled row and any row left empty after cleaning. [V]

### 5.4 Train / validation / test splits [V]

**No split has been made.** The proposal plans a per-era stratified 70 / 15 / 15 split with six half-year eras from 2020 to 2025 and about 2,000 annotated samples per era. None of that exists yet: there is no `split` column in any file.

### 5.5 Era (time) distribution [V]

The proposal's six eras (2020 to 2025, half-year) cannot be filled from this data. Rows by era, all usable rows versus human-labeled rows (the era uses the date parser in `datefmt.py`):

| era | all usable rows | human-labeled | Positive | Neutral | Negative | binary (Pos+Neg) |
|---|---|---|---|---|---|---|
| 2020-H1 | 4 | 2 | 0 | 1 | 1 | 1 |
| 2020-H2 | 100 | 47 | 16 | 15 | 16 | 32 |
| 2021-H1 | 20 | 12 | 6 | 3 | 3 | 9 |
| 2021-H2 | 41 | 37 | 12 | 10 | 15 | 27 |
| 2022-H1 | 61 | 53 | 19 | 21 | 13 | 32 |
| 2022-H2 | 34 | 23 | 10 | 10 | 3 | 13 |
| 2022-year-only | 594 | 143 | 85 | 34 | 24 | 109 |
| 2023-H1 | 1,343 | 475 | 89 | 195 | 191 | 280 |
| 2023-H2 | 708 | 92 | 36 | 23 | 33 | 69 |
| 2024-H1 | 1,005 | 119 | 43 | 27 | 49 | 92 |
| 2024-H2 | 1,531 | 100 | 33 | 30 | 37 | 70 |
| 2025-H1 | 1,739 | 106 | 40 | 24 | 42 | 82 |
| 2025-H2 | 2,208 | 191 | 101 | 39 | 51 | 152 |
| 2026-H1 | 6,146 | 88 | 47 | 14 | 27 | 74 |
| 2026-H2 | 5,604 | 614 | 251 | 101 | 262 | 513 |
| undated | 22 | 6 | 5 | 1 | 0 | 5 |
| **TOTAL** | 21,160 | 2,108 | 793 | 548 | 767 | 1,560 |

By calendar year:

| year | all usable rows | human-labeled |
|---|---|---|
| undated | 22 | 6 |
| 2020 | 104 | 49 |
| 2021 | 61 | 49 |
| 2022 | 689 | 219 |
| 2023 | 2,051 | 567 |
| 2024 | 2,536 | 219 |
| 2025 | 3,947 | 297 |
| 2026 | 11,750 | 702 |
| **TOTAL** | 21,160 | 2,108 |

* The 2022 rows are mostly year-only (594 of 689), so 2022 cannot be split into halves.
* 2020 to 2022 together have only 317 human labels.
* The proposal's window ends in 2025, while 11,750 of 21,160 usable rows are dated 2026. Options I put to you, unanswered: shift the window to 2023 to 2026, merge 2020 to 2022 into one legacy era, or use data-driven eras.

All labeled rows (human + emoji) by year, as they appear in `sheng_labeled_dataset.csv`:

| year | emoji_heuristic (unverified) | human | Total |
|---|---|---|---|
| 2020 | 5 | 49 | 54 |
| 2021 | 0 | 49 | 49 |
| 2022 | 0 | 219 | 219 |
| 2023 | 108 | 567 | 675 |
| 2024 | 167 | 219 | 386 |
| 2025 | 276 | 297 | 573 |
| 2026 | 370 | 702 | 1,072 |
| undated | 2 | 6 | 8 |
| **TOTAL** | 928 | 2,108 | 3,036 |

Usable-data funnel (rows and human labels at each filter) [V]:

| subset | rows | binary human labels (Pos+Neg) | all human labels |
|---|---|---|---|
| All rows in cleaned_population.csv | 21,245 | 1,560 | 2,108 |
| ...with non-empty text_clean | 21,160 | 1,560 | 2,108 |
| ...with a parseable date | 21,138 | 1,555 | 2,102 |
| ...dated 2023-2026 | 20,284 | 1,332 | 1,785 |
| ...dated 2020-2025 (the proposal window) | 9,388 | 968 | 1,400 |
| ...containing a Sheng-lexicon term (contains_sheng) | 5,502 | 931 | 1,235 |
| ...Sheng and dated 2023-2026 | 5,176 | 761 | 992 |

Earlier in the chat I also reported rows after "excluding glossary and definition-style rows" (19,523 rows, 977 binary labels) and the Sheng subset of those (4,835 rows, 495 binary labels). The glossary rule was in a lost script, so these are **[S] and not reproduced here**.

## 6. Statistics and examples

### 6.1 Text length [V]

Characters and whitespace-separated words, measured on `text_clean`:

| subset | rows | chars_mean | chars_median | chars_p90 | chars_max | words_mean | words_median | words_p90 | words_max |
|---|---|---|---|---|---|---|---|---|---|
| all usable rows (text_clean) | 21,160 | 85.2 | 64 | 195 | 355 | 14.3 | 11 | 31 | 59 |
| labeled dataset (human + emoji) | 3,036 | 76.9 | 62 | 148 | 350 | 13.2 | 10 | 26 | 58 |
| human-labeled | 2,108 | 67.6 | 56 | 132 | 305 | 11.6 | 10 | 23 | 55 |
| human Positive | 793 | 67.6 | 55 | 130 | 278 | 11.7 | 9 | 23 | 55 |
| human Neutral | 548 | 55.9 | 43 | 109 | 305 | 9.8 | 8 | 19 | 52 |
| human Negative | 767 | 76.1 | 66 | 135 | 280 | 12.7 | 11 | 23 | 53 |
| emoji-heuristic labeled | 928 | 98.1 | 81 | 204 | 350 | 16.8 | 14 | 35 | 58 |
| sheng_unlabeled_dataset | 4,003 | 109.8 | 91 | 230 | 355 | 18.5 | 16 | 37 | 55 |

The longest text is 355 characters. That ceiling suggests a source-side length cap; I did not check which sources set it.

### 6.2 Vocabulary [V]

Tokens are lower-cased runs of letters, digits and apostrophes. There is no stemming, spelling normalisation or stop-word removal.

| subset | tokens | types | hapax | type_token_ratio |
|---|---|---|---|---|
| all usable rows | 310,561 | 33,597 | 16,452 | 0.108 |
| labeled dataset (human + emoji) | 40,271 | 10,178 | 5,710 | 0.253 |
| human-labeled only | 24,311 | 7,586 | 4,580 | 0.312 |
| sheng_unlabeled_dataset | 75,832 | 13,405 | 7,130 | 0.177 |

The 20 most frequent tokens in the labeled dataset:

| token (labeled dataset) | count |
|---|---|
| na | 850 |
| the | 645 |
| ni | 536 |
| ya | 468 |
| a | 457 |
| to | 391 |
| kwa | 356 |
| in | 292 |
| and | 271 |
| of | 238 |
| is | 230 |
| hii | 212 |
| we | 199 |
| rt | 181 |
| this | 171 |
| i | 167 |
| you | 162 |
| for | 162 |
| kama | 159 |
| on | 153 |

The list is dominated by Swahili and English function words (`na`, `ni`, `ya`, `kwa`, `the`, `to`), and `rt` appears 181 times, which shows retweet markers left in the text.

### 6.3 Examples per class [V]

These come from `sheng_labeled_dataset.csv`, drawn at random (seed 11) from rows with no name-like leading text and no capitalised word inside a sentence (to avoid personal names). They carry the cleaned text only, with no author. A name inside a sentence could still slip through, so check each before quoting it in the thesis.

| label | source | id | date_posted | text |
|---|---|---|---|---|
| Positive | human | R004928 | 08/05/2023 | rt : there is a lot of tricks at play for many of them however despite all pia wanajituma sio kazi kidogo cooking for the |
| Positive | human | R002183 | 2022 | This guy is doing something that has never been don in ksm. Congratulations kwa investor |
| Positive | human | R019792 | 08/05/2023 | RT : Niko nditni kama kawaida. Join the crew for awesome automotive conversations at |
| Neutral | human | R003095 | 07/09/2026 | the more i try to understand this post , the more i get confused and confused again. anyway, tujichunge hii weeknd |
| Neutral | human | R004772 | 23/04/2025 | rt : sijui kama anangam lakini niyeye anadai tubanje |
| Neutral | human | R011031 | 06/06/2023 | Niaje mkuu ready made ama anything printable??? |
| Negative | human | R019861 | 07/04/2023 | Fathako mchafu mbaka ana mtaro kwa kifua |
| Negative | human | R007136 | 29/11/2023 | rt : jana niliweka britney 2k ndio iweze kufika kwa my favorite petrol station guage ikabaki same spot ni kama waliniwekea hewa. am |
| Negative | human | R003183 | 07/09/2026 | you are right sir ,, hapo tunanyongwa vibaya sana esp hio njia ya kwenda km (kenyatta market) |
| Positive | emoji heuristic | R010375 | 27/10/2024 | Just a reminder. Book your staycations mapema . Usituite fek friends tukikuacha nbo alone . natumai umepata confirmed |
| Positive | emoji heuristic | R016515 | 26/05/2023 | Ni kama tulicheza na NZOIA SUGAR FC jana |
| Negative | emoji heuristic | R007866 | 03/10/2025 | solar water heater will have you bathing on cold water every morning ffs |
| Negative | emoji heuristic | R012786 | 03/02/2025 | Mko na hone bar na bado naisha bed sita sieet bring work nihame sasa |

Unlabeled examples from `sheng_unlabeled_dataset.csv`:

| id | date_posted | text | sheng_words_used | sheng_meanings |
|---|---|---|---|---|
| R022117 | 22/11/2025 | Hiyo barber shop sitaikosa ,mangai madem wameiva ajab | madem | madem = girls, ladies (plural of dem) |
| R023226 | 16/08/2026 | Early gen z walikuwa na weird names wdym moshi ya sembe ama makwapa ya kuku joh | joh | joh = man (as in "it's cool, man") |
| R003871 | 30/07/2026 | kudonjo kwa club na tukaanza chapa usiku kucha. | kudonjo | kudonjo = to enter (from Luo "donjo" = to enter) |
| R016890 | 03/07/2026 | Be it ukweli ama stories hiwezi kula deposit mdosi akuache haha heri ufanye job ukatwe haikuangi rahisi ivi haha | mdosi | mdosi = rich person (web-sourced) |

## 7. Decisions and limitations

### 7.1 Decisions made, and why

| Decision | Why | Source |
|---|---|---|
| Keep three classes | You labelled Positive / Neutral / Negative from the start | S |
| Never use `pg` / `none pg` as sentiment | It is a content-appropriateness rating (you confirmed: `pg` = not appropriate for children) | S, V |
| Human label overrides the emoji label | Human labels are verified, emoji labels are not | V |
| Keep the first occurrence of duplicate text and count the copies | Avoids duplicated training or test rows while keeping a frequency signal | S |
| Dedup never drops a labeled row in favour of an unlabeled copy | A labeled row is more valuable than an earlier unlabeled twin | S |
| Keep emoji-only comments in the master | They carry sentiment signal | S |
| Strip emoji, hashtags, mentions, URLs, phone numbers and non-ASCII from `text_clean` | You asked for emoji, hashtags, PII and non-ASCII removal; this is the annotator-facing and export text | S |
| Keep risky lexicon words active | Your instruction | S |
| Sheng flag by lexicon match | No language ID exists for Sheng | V |
| Day-first reading of `dd/mm/yyyy`; month-first only for the `m/d/yyyy hh:mm` source | Tested on the data (no second part above 12) | S |
| Stable row ids and an additive registry | Labels must survive new uploads | S |
| Leave ambiguous dates blank (`02/05`, `0`) | Cannot be placed safely | V |
| Treat meaning placeholders and "not verified" as no meaning | They are not meanings | V |
| Tap-labeling queue: natural text only, eras round-robin | Balance across eras and avoid glossary rows | S |

### 7.2 Known problems

* **Too few labels.** 1,560 human binary labels against the proposal's ~12,000. Only 1,785 human labels fall in 2023 to 2026, and fewer than 100 fall in some single half-years.
* **Era design unworkable as written.** 2020 to 2022 are almost empty, 2026 dominates, and 2022 is mostly year-only. 2026 dates may be scrape dates.
* **Class imbalance.** Human: Positive 793, Neutral 548, Negative 767 (fairly even). Emoji labels: 842 Positive to 86 Negative, no Neutral, so mixing them skews the data strongly Positive.
* **Label provenance is uneven.** Human labels come mostly from the curated Sheng datasets (714), `twitter_scrape` (352) and `Matwana_2022` (143), and only 576 of the 15,804 Twitter/X rows. A model trained on them is not trained on a random sample of the corpus.
* **No agreement score and no guideline.** Label quality is unmeasured. Short texts, sarcasm and code-switching make single-annotator noise likely. Neutral is the shortest class (median 43 characters).
* **Sheng flag is noisy.** Ordinary words (`so`, `time`) inflate it; rows that match only on these are not necessarily Sheng. Misses are also likely: variants and inflections of lexicon words (`mashashola`, `masenke`, `tembea`, `mabei`) and Shembeteng forms (syllable-inserted slang) are not matched. A pattern rule for Shembeteng was offered and not built.
* **Text noise.** Retweet markers (`rt`), news-headline and promotional rows, and glossary-style rows remain in the data, and many rows are mostly English or Swahili with little Sheng (the share was not measured). Accented letters were deleted, and deleting emoji removes sentiment cues from `text_clean`.
* **Anonymisation is partial** (section 3.7). The master file contains author handles.
* **Provenance and compliance unknown** (section 1.2): ethics approval, scraping method and platform terms are not documented here.
* **Truncated and corrupted sources.** 125 truncated rows were removed. The Instagram export recovered only 11 captions and may still hold merged or truncated text.
* **Meanings are uneven.** 109 lexicon entries came from a dictionary page without checked meanings, and 51 have no meaning. `kudade` was recorded as "to cum" but the corpus use reads like an adjective meaning great; I asked and you never answered. `rotejo`, `sagara`, `tishoo` still have no confirmed meaning. [S]
* **Lost reproducibility.** The original pipeline cannot be re-run from scripts (section 8.2). Treat `cleaned_population.csv` as the source of truth, and archive it.

### 7.3 Open decisions waiting for you

1. Three classes versus binary (amend the proposal or drop Neutral).
2. The era window (2023 to 2026, a legacy era, or data-driven eras).
3. Whether to remove `so`, `k` and `red` from the Sheng filter.
4. Whether to transliterate accents instead of deleting them.
5. Whether to convert the data to the schema I proposed (instances / labels / weak labels / splits / lexicon / exclusions, with `author_hash`, Parquet and DVC).
6. Whether the `pg` / `none pg` flag becomes a `content_rating` column.
7. An ethics-approval reference and the collection method for each source, for the thesis methodology chapter.

## 8. Code

### 8.1 Scripts reproduced in full

These are exact copies of the files or of the commands I ran, taken from disk or from the saved transcript. They are the **recreated** versions I wrote after resets, so they may differ in detail from the originals the earlier numbers came from.

#### 8.1.1 `filter_sheng_only.py` (the Sheng flag; copied from the session summary, not from disk)

```python
import pandas as pd, re
lex = pd.read_csv('/home/claude/pipeline/sheng_lexicon_clean.csv')['term'].str.strip().str.lower().tolist()
def _term_pat(t):
    e = re.escape(t)
    return e + r'(?!\s*(?:[1-4]|one|two|three|four)\b)' if t == 'form' else e
pat = re.compile(r'\b(' + '|'.join(_term_pat(t) for t in sorted(lex, key=len, reverse=True)) + r')\b', re.IGNORECASE)
df = pd.read_csv('/mnt/user-data/outputs/cleaned_population.csv')
df = df.drop(columns=[c for c in ['contains_sheng','matched_sheng_terms'] if c in df.columns])
hits = df['text_plain_emoji'].fillna('').apply(lambda s: sorted(set(m.lower() for m in pat.findall(s))))
df['contains_sheng'] = hits.apply(bool)
df['matched_sheng_terms'] = hits.apply('; '.join)
print('lexicon terms:', len(lex)); print('rows flagged as containing Sheng:', int(df['contains_sheng'].sum()), 'of', len(df))
df.to_pickle('/home/claude/pipeline/sheng_flagged.pkl')
```

#### 8.1.2 `add_word.py` (add or update a lexicon term, re-flag, rewrite outputs; final version used for `buya`)

```python
import pandas as pd, re, sys
term, meaning = sys.argv[1].lower(), sys.argv[2]
O='/mnt/user-data/outputs/'
wl=pd.read_csv(O+'sheng_words_list.csv')
old=wl[wl.term.str.lower()==term].documented_meaning.tolist()
print('already on list:',bool(old),'| previous meaning:',old)
df=pd.read_csv(O+'cleaned_population.csv')
for c in ['contains_sheng','matched_sheng_terms']:
    if c in df.columns: df=df.drop(columns=c)
wl=wl[wl.term.str.lower()!=term]
pat_new=re.compile(r'\b'+re.escape(term)+r'\b',re.I)
n_new=int(df['text_plain_emoji'].fillna('').apply(lambda s: bool(pat_new.search(s))).sum())
wl=pd.concat([wl,pd.DataFrame([{'term':term,'documented_meaning':meaning+' (user-confirmed)','rows_in_corpus_matching':n_new}])]).sort_values('term')
lex=sorted(set(wl.term.str.strip().str.lower()))
def tp(t):
    e=re.escape(t); return e+r'(?!\s*(?:[1-4]|one|two|three|four)\b)' if t=='form' else e
pat=re.compile(r'\b('+'|'.join(tp(t) for t in sorted(lex,key=len,reverse=True))+r')\b',re.I)
hits=df['text_plain_emoji'].fillna('').apply(lambda s: sorted(set(m.lower() for m in pat.findall(s))))
df['contains_sheng']=hits.apply(bool); df['matched_sheng_terms']=hits.apply('; '.join)
wl.to_csv(O+'sheng_words_list.csv',index=False)
df[df.contains_sheng].drop(columns=['contains_sheng']).to_csv(O+'cleaned_population_sheng_only.csv',index=False)
df.to_csv(O+'cleaned_population.csv',index=False)
print('lexicon terms:',len(lex),'| corpus rows matching',term+':',n_new,'| Sheng-only rows:',int(df.contains_sheng.sum()))
```

#### 8.1.3 `add_word` first version (used for `chwepe`, 2026-10-08; it also read `sheng_lexicon_clean.csv`, which no longer exists)

```python
import pandas as pd, re, sys
term, meaning = sys.argv[1].lower(), sys.argv[2]
O='/mnt/user-data/outputs/'
wl=pd.read_csv(O+'sheng_words_list.csv')
print('already on list:', term in set(wl.term.str.lower()))
df=pd.read_csv(O+'cleaned_population.csv')
pat_new=re.compile(r'\b'+re.escape(term)+r'\b',re.I)
n_new=int(df['text_plain_emoji'].fillna('').apply(lambda s: bool(pat_new.search(s))).sum())
wl=wl[wl.term.str.lower()!=term]
wl=pd.concat([wl,pd.DataFrame([{'term':term,'documented_meaning':meaning+' (user-confirmed)','rows_in_corpus_matching':n_new}])]).sort_values('term')
lex=sorted(set(wl.term.str.strip().str.lower()))
pd.DataFrame({'term':lex}).to_csv('sheng_lexicon_clean.csv',index=False)
def tp(t):
    e=re.escape(t); return e+r'(?!\s*(?:[1-4]|one|two|three|four)\b)' if t=='form' else e
pat=re.compile(r'\b('+'|'.join(tp(t) for t in sorted(lex,key=len,reverse=True))+r')\b',re.I)
df=df.drop(columns=['contains_sheng','matched_sheng_terms'])
hits=df['text_plain_emoji'].fillna('').apply(lambda s: sorted(set(m.lower() for m in pat.findall(s))))
df['contains_sheng']=hits.apply(bool); df['matched_sheng_terms']=hits.apply('; '.join)
wl.to_csv(O+'sheng_words_list.csv',index=False)
df[df.contains_sheng].drop(columns=['contains_sheng']).to_csv(O+'cleaned_population_sheng_only.csv',index=False)
df.to_csv(O+'cleaned_population.csv',index=False)
print('lexicon terms:',len(lex),'| corpus rows matching',term+':',n_new,'| Sheng-only rows:',int(df.contains_sheng.sum()))
```

#### 8.1.4 `label_tool.py` (tap-labeling backend)

```python
import pandas as pd, sys, json, os, datetime
Q='/mnt/user-data/outputs/label_queue.csv'; POP='/mnt/user-data/outputs/cleaned_population.csv'
LOG='/mnt/user-data/outputs/labeling_session_log.csv'; PEND='/mnt/user-data/outputs/label_pending.json'
def next_batch(n=3):
    q=pd.read_csv(Q); pend=q[q['status']=='pending'].head(n); ids=pend['row_id'].tolist()
    q.loc[q['row_id'].isin(ids),'status']='shown'; q.to_csv(Q,index=False); json.dump(ids,open(PEND,'w'))
    for i,r in enumerate(pend.itertuples(),1): print(f'{i}|{r.row_id}|{r.era}|{r.text_clean}')
    print('remaining in queue:',int((q['status']=='pending').sum()))
def apply(pairs):
    q=pd.read_csv(Q); pop=pd.read_csv(POP)
    log=pd.read_csv(LOG) if os.path.exists(LOG) else pd.DataFrame(columns=['row_id','label','era','time'])
    now=datetime.datetime.utcnow().isoformat(timespec='seconds'); n_lab=0
    for rid,lab in pairs:
        lab=lab.lower(); era=q.loc[q.row_id==rid,'era'].values[0] if (q.row_id==rid).any() else ''
        if lab=='skip': q.loc[q.row_id==rid,'status']='skipped'
        else:
            m=pop.row_id==rid; pop.loc[m,'existing_label']=lab.capitalize()
            pop.loc[m,'existing_label_type']='manual_sentiment_label (Positive/Negative/Neutral)'
            q.loc[q.row_id==rid,'status']='labeled'; n_lab+=1
        log.loc[len(log)]=[rid,lab,era,now]
    pop.to_csv(POP,index=False); q.to_csv(Q,index=False); log.to_csv(LOG,index=False)
    print(f'applied {n_lab} labels, {len(pairs)-n_lab} skipped | total human labels now: {(pop["existing_label_type"]=="manual_sentiment_label (Positive/Negative/Neutral)").sum()}')
if __name__=='__main__':
    if sys.argv[1]=='next': next_batch(int(sys.argv[2]))
    else: apply([tuple(p.split(':')) for p in sys.argv[2].split(',')])
```

#### 8.1.5 `datefmt.py` (date parser used for the year and era tables in this record)

```python
import re
MON={m:i for i,m in enumerate(['jan','feb','mar','apr','may','jun','jul','aug','sep','oct','nov','dec'],1)}
def parse(s):
    """returns (year, month|None, day|None, form) ; day-first for a/b/yyyy; month-first only for 'm/d/yyyy hh:mm'"""
    s=str(s).strip()
    m=re.match(r'^(\d{4})$',s)
    if m: return int(m.group(1)),None,None,'year'
    m=re.match(r'^(\d{4})-(\d{1,2})-(\d{1,2})',s)
    if m: y,mo,d=map(int,m.groups()); return y,mo,d,'iso'
    m=re.match(r'^(\d{1,2})/(\d{1,2})/(\d{4})$',s)
    if m: d,mo,y=map(int,m.groups()); return y,mo,d,'dmy'
    m=re.match(r'^(\d{1,2})/(\d{1,2})/(\d{4})\s+\d{1,2}:\d{2}$',s)
    if m: mo,d,y=map(int,m.groups()); return y,mo,d,'mdy_time'
    m=re.match(r'^(\d{1,2})-([A-Za-z]{3})-(\d{2})$',s)
    if m and m.group(2).lower() in MON: return 2000+int(m.group(3)),MON[m.group(2).lower()],int(m.group(1)),'d-Mon-yy'
    m=re.match(r'^[A-Za-z]{3}\s+([A-Za-z]{3})\s+(\d{1,2})\s+(\d{4})$',s)
    if m and m.group(1).lower() in MON: return int(m.group(3)),MON[m.group(1).lower()],int(m.group(2)),'Dow Mon d yyyy'
    m=re.match(r'^([A-Za-z]{3})\s+(\d{1,2})\s+(\d{4})$',s)
    if m and m.group(1).lower() in MON: return int(m.group(3)),MON[m.group(1).lower()],int(m.group(2)),'Mon d yyyy'
    m=re.match(r'^([A-Za-z]{3,9})\s+(\d{1,2}),\s*(\d{4})$',s)               # January 15, 2023
    if m and m.group(1)[:3].lower() in MON: return int(m.group(3)),MON[m.group(1)[:3].lower()],int(m.group(2)),'Month d, yyyy'
    m=re.match(r'^(\d{1,2})/(\d{2})$',s)
    if m and 1<=int(m.group(1))<=12 and 20<=int(m.group(2))<=30: return 2000+int(m.group(2)),int(m.group(1)),None,'mm/yy'
    return None,None,None,'unparsed'
def era(y,m):
    if y is None: return 'undated'
    if m is None: return f'{y}-year-only'
    return f'{y}-H{1 if m<=6 else 2}'
```

#### 8.1.6 `build_unlabeled.py` (builds `sheng_unlabeled_dataset.csv`; also holds the date converter `fmt()` and the meaning rules)

```python
import pandas as pd, re, datetime as dt
O='/mnt/user-data/outputs/'
HUM='manual_sentiment_label (Positive/Negative/Neutral)'
p=pd.read_csv(O+'cleaned_population.csv')
wl=pd.read_csv(O+'sheng_words_list.csv')
MON={m:i for i,m in enumerate(['jan','feb','mar','apr','may','jun','jul','aug','sep','oct','nov','dec'],1)}
def fmt(s):
    s=str(s).strip()
    m=re.match(r'^(\d{4})$',s)
    if m: return m.group(1)
    m=re.match(r'^(\d{4})-(\d{1,2})-(\d{1,2})',s)                    # ISO, with or without time
    if m: y,mo,d=map(int,m.groups()); return f'{d:02d}/{mo:02d}/{y}'
    m=re.match(r'^(\d{1,2})/(\d{1,2})/(\d{4})$',s)                   # day-first
    if m: d,mo,y=map(int,m.groups()); return f'{d:02d}/{mo:02d}/{y}'
    m=re.match(r'^(\d{1,2})/(\d{1,2})/(\d{4})\s+\d{1,2}:\d{2}$',s)   # month-first with time (sheng_mtaa_twitter)
    if m: mo,d,y=map(int,m.groups()); return f'{d:02d}/{mo:02d}/{y}'
    m=re.match(r'^(\d{1,2})-([A-Za-z]{3})-(\d{2})$',s)               # 4-Jul-24
    if m and m.group(2).lower() in MON: return f'{int(m.group(1)):02d}/{MON[m.group(2).lower()]:02d}/20{m.group(3)}'
    m=re.match(r'^[A-Za-z]{3}\s+([A-Za-z]{3})\s+(\d{1,2})\s+(\d{4})$',s)  # Wed Jan 21 2026
    if m and m.group(1).lower() in MON: return f'{int(m.group(2)):02d}/{MON[m.group(1).lower()]:02d}/{m.group(3)}'
    m=re.match(r'^([A-Za-z]{3})\s+(\d{1,2})\s+(\d{4})$',s)           # jul 8 2020
    if m and m.group(1).lower() in MON: return f'{int(m.group(2)):02d}/{MON[m.group(1).lower()]:02d}/{m.group(3)}'
    m=re.match(r'^([A-Za-z]{3,9})\s+(\d{1,2}),\s*(\d{4})$',s)               # January 15, 2023
    if m and m.group(1)[:3].lower() in MON: return f'{int(m.group(2)):02d}/{MON[m.group(1)[:3].lower()]:02d}/{m.group(3)}'
    m=re.match(r'^(\d{1,2})/(\d{2})$',s)                             # mm/yy only when yy is plausible (20-30)
    if m and 1<=int(m.group(1))<=12 and 20<=int(m.group(2))<=30: return f'{int(m.group(1)):02d}/20{m.group(2)}'
    return ''
meaning={}
for t,mn in zip(wl.term.str.lower(),wl.documented_meaning):
    if isinstance(mn,str) and mn.strip():
        mn=mn.strip()
        if re.search(r'no meaning recorded|^not verified',mn,re.I): continue
        mn=re.sub(r'\s*\[RISKY[^\]]*\]','',mn)
        meaning[t]=re.sub(r'\s*\(user-confirmed\)\s*$','',mn).strip()
d=p[p.contains_sheng & (p.existing_label_type!=HUM) & p.emoji_sentiment.isna() & ~p.is_empty_after_pii_clean.astype(bool)].copy()
d['date_posted']=d.date_raw.apply(fmt)
d['sheng_words_used']=d.matched_sheng_terms.fillna('')
def mean(ts): return '; '.join(f'{t} = {meaning[t]}' for t in [x.strip() for x in ts.split(';')] if t in meaning)
d['sheng_meanings']=d.sheng_words_used.apply(mean)
out=d.drop(columns=['text']).rename(columns={'row_id':'id','text_clean':'text'})[['id','date_posted','text','sheng_words_used','sheng_meanings']]
out['label']=''
out.to_csv(O+'sheng_unlabeled_dataset.csv',index=False,encoding='utf-8')
print('rows:',len(out))
print('date formats -> year only:',int(out.date_posted.str.fullmatch(r'\d{4}').sum()),'| mm/yyyy:',int(out.date_posted.str.fullmatch(r'\d{2}/\d{4}').sum()),'| dd/mm/yyyy:',int(out.date_posted.str.fullmatch(r'\d{2}/\d{2}/\d{4}').sum()),'| blank:',int((out.date_posted=='').sum()))
print('rows with >=1 documented meaning:',int((out.sheng_meanings!='').sum()))
print('unparsed raw dates:',d[out.date_posted.values==''].date_raw.value_counts().to_dict())
```

#### 8.1.7 Build of `sheng_labeled_dataset.csv` (run inline on 2026-11-14; it executes the top half of `build_unlabeled.py`)

```python
import pandas as pd, re
src=open('build_unlabeled.py').read().split("d=p[")[0]
exec(src)   # defines fmt(), meaning{}, p, HUM, O
def mean(ts): return '; '.join(f'{t} = {meaning[t]}' for t in [x.strip() for x in str(ts).split(';')] if t in meaning)
human=p.existing_label_type==HUM
emoji=p.emoji_sentiment.notna()&~human
d=p[(human|emoji)&~p.is_empty_after_pii_clean.astype(bool)].copy()
d['label']=d.existing_label.where(human[d.index], d.emoji_sentiment.str.capitalize())
d['label_source']=['human' if human[i] else 'emoji_heuristic (unverified)' for i in d.index]
d['date_posted']=d.date_raw.apply(fmt)
d['contains_sheng']=d.contains_sheng.map({True:'yes',False:'no'})
d['sheng_words_used']=d.matched_sheng_terms.fillna('')
d['sheng_meanings']=d.sheng_words_used.apply(mean)
out=d.drop(columns=['text']).rename(columns={'row_id':'id','text_clean':'text'})[['id','date_posted','text','contains_sheng','sheng_words_used','sheng_meanings','label','label_source']]
out=out.sort_values('id')
out.to_csv(O+'sheng_labeled_dataset.csv',index=False,encoding='utf-8')
print('rows:',len(out),'| unique ids:',out.id.is_unique)
print(pd.crosstab(out.label_source,out.label,margins=True).to_string())
print('contains sheng:',out.contains_sheng.value_counts().to_dict())
print('blank dates:',int((out.date_posted=='').sum()),'| empty text:',int((out.text.str.strip()=='').sum()),'| non-ascii:',int(out.text.apply(lambda t:any(ord(c)>127 for c in t)).sum()))
print('human labeled rows dropped for empty text:',int((human&p.is_empty_after_pii_clean.astype(bool)).sum()),'| emoji rows dropped:',int((emoji&p.is_empty_after_pii_clean.astype(bool)).sum()))
pd.set_option('display.width',200,'display.max_colwidth',60)
print(out.sample(6,random_state=5).to_string(index=False))
```

#### 8.1.8 Inspection of raw date shapes (2026-11-09)

```python
import pandas as pd, re
p=pd.read_csv('/mnt/user-data/outputs/cleaned_population.csv',usecols=['row_id','source_group','date_raw','existing_label_type','emoji_sentiment','contains_sheng'])
HUM='manual_sentiment_label (Positive/Negative/Neutral)'
d=p[p.contains_sheng & (p.existing_label_type!=HUM)]
print(len(d), 'sheng rows without human label;', int(d.emoji_sentiment.notna().sum()),'of them emoji-labeled')
def shape(s):
    s=str(s); s=re.sub(r'[A-Za-z]{3,}','Mon',s); return re.sub(r'\d','9',s)
sh=d.date_raw.apply(shape).value_counts()
print(sh.head(30).to_string())
for k in sh.index[:30]:
    print(repr(k), '->', d[d.date_raw.apply(shape)==k].date_raw.iloc[0], '|', d[d.date_raw.apply(shape)==k].source_group.iloc[0])
```

#### 8.1.9 Labels-by-year analyses run in chat (2026-11-09). The first two were earlier drafts, the third is the version whose table I reported

```python
# --- first pass (regex for 4-digit years only; left 152 rows undated)
import pandas as pd, re
p=pd.read_csv('/mnt/user-data/outputs/cleaned_population.csv',usecols=['row_id','date_raw','existing_label','existing_label_type','source_group'])
h=p[p.existing_label_type=='manual_sentiment_label (Positive/Negative/Neutral)'].copy()
print('human labels:',len(h))
def year(s):
    s=str(s)
    m=re.findall(r'(?<!\d)(20[12]\d)(?!\d)',s)
    if m: return int(m[0])
    m=re.match(r'^\s*\d{1,2}/(\d{2})\s*$',s)   # mm/yy
    if m: return 2000+int(m.group(1))
    return None
h['year']=h.date_raw.apply(year)
print('undated/unparsed:',h.year.isna().sum())
print(h[h.year.isna()].date_raw.astype(str).value_counts().head(8).to_string())
t=pd.crosstab(h.year.fillna(0).astype(int).replace(0,'undated'),h.existing_label,margins=True)
print(t.to_string())
print()
print('by source_group (top):'); print(h.source_group.value_counts().head(8).to_string())

# --- second pass (adds dd-Mon-yy and mm/yy)
import pandas as pd, re
p=pd.read_csv('/mnt/user-data/outputs/cleaned_population.csv',usecols=['row_id','date_raw','existing_label','existing_label_type'])
h=p[p.existing_label_type=='manual_sentiment_label (Positive/Negative/Neutral)'].copy()
def year(s):
    s=str(s)
    m=re.findall(r'(?<!\d)(20[12]\d)(?!\d)',s)
    if m: return int(m[0]), 'full'
    m=re.match(r'^\s*\d{1,2}-[A-Za-z]{3}-(\d{2})\s*$',s)
    if m: return 2000+int(m.group(1)), 'dd-Mon-yy'
    m=re.match(r'^\s*\d{1,2}/(\d{2})\s*$',s)
    if m: return 2000+int(m.group(1)), 'mm/yy'
    return None, 'none'
r=h.date_raw.apply(year); h['year']=[x[0] for x in r]; h['how']=[x[1] for x in r]
print(h.how.value_counts().to_dict())
print(h[h.year.isna()].date_raw.astype(str).value_counts().head(10).to_string())
print(h[h.year==2005].date_raw.tolist())
t=pd.crosstab(h.year.fillna(0).astype(int).replace(0,'undated'),h.existing_label,margins=True)
print(t.to_string())

# --- third pass (adds emoji-heuristic labels per year)
import pandas as pd, re
p=pd.read_csv('/mnt/user-data/outputs/cleaned_population.csv',usecols=['row_id','date_raw','existing_label','existing_label_type','emoji_sentiment','is_empty_after_pii_clean'])
HUM='manual_sentiment_label (Positive/Negative/Neutral)'
def year(s):
    s=str(s)
    if s.strip()=='02/05': return None
    m=re.findall(r'(?<!\d)(20[12]\d)(?!\d)',s)
    if m: return int(m[0])
    m=re.match(r'^\s*\d{1,2}-[A-Za-z]{3}-(\d{2})\s*$',s)
    if m: return 2000+int(m.group(1))
    m=re.match(r'^\s*\d{1,2}/(\d{2})\s*$',s)
    if m: return 2000+int(m.group(1))
    return None
p['year']=p.date_raw.apply(year)
p['human']=p.existing_label_type==HUM
print(p.emoji_sentiment.value_counts(dropna=False).to_dict())
print('emoji-labeled rows that ALSO have a human label:',int((p.emoji_sentiment.notna()&p.human).sum()))
print(p.existing_label_type.value_counts().to_dict())
emo=p[p.emoji_sentiment.notna()&~p.human]
hum=p[p.human]
pg=p[p.existing_label_type.astype(str).str.contains('PG',case=False)&~p.human]
print('pg-type rows:',len(pg), pg.existing_label.value_counts().to_dict())
def tab(d,col): return d.groupby(d.year.fillna(0).astype(int).replace(0,'undated'))[col].agg(lambda s:s.str.lower().value_counts().to_dict()).to_dict() if len(d) else {}
yrs=sorted(set(p.year.dropna().astype(int)))+['undated']
rows=[]
for y in yrs:
    sel=lambda d: d[(d.year.isna() if y=='undated' else d.year==y)]
    h=sel(hum); e=sel(emo)
    rows.append(dict(year=y,
      human=len(h),
      emoji_pos=int((e.emoji_sentiment.str.lower()=='positive').sum()),
      emoji_neg=int((e.emoji_sentiment.str.lower()=='negative').sum()),
      emoji_total=len(e)))
t=pd.DataFrame(rows); t['all_sentiment_labels']=t.human+t.emoji_total
t=t[t.all_sentiment_labels>0]
t.loc[len(t)]=['TOTAL']+[t[c].sum() for c in t.columns[1:]]
print(t.to_string(index=False))
```

#### 8.1.10 `build_record.py` (this document's generator; every table in sections 2 to 6 is computed by it)

`````python
#!/usr/bin/env python3
"""Builds /mnt/user-data/outputs/sheng_data_record.md.
Every table of counts is computed from the files in /mnt/user-data/outputs and /mnt/user-data/uploads
at run time; only the prose is hand-written. Run:  python3 -W ignore -I build_record.py
"""
import pandas as pd, re, json, glob, os, sys, collections, datetime, numbers
sys.path.insert(0, os.path.dirname(os.path.abspath(__file__)))
from datefmt import parse, era

O = '/mnt/user-data/outputs/'
HERE = os.path.dirname(os.path.abspath(__file__))
HUM = 'manual_sentiment_label (Positive/Negative/Neutral)'


def md(df, index=True):
    d = df.reset_index() if index else df.copy()
    cols = [str(c) for c in d.columns]
    out = ['| ' + ' | '.join(cols) + ' |', '|' + '|'.join(['---'] * len(cols)) + '|']
    for r in d.itertuples(index=False):
        out.append('| ' + ' | '.join(f'{v:,}' if isinstance(v, numbers.Integral) and not isinstance(v, bool) else str(v) for v in r) + ' |')
    return '\n'.join(out)


k = pd.read_csv(O + 'cleaned_population.csv')
e = pd.read_csv(O + 'excluded_rows_log.csv')
lab = pd.read_csv(O + 'sheng_labeled_dataset.csv')
unl = pd.read_csv(O + 'sheng_unlabeled_dataset.csv')
sd = pd.read_csv(O + 'sentiment_dataset.csv')
so = pd.read_csv(O + 'cleaned_population_sheng_only.csv')
q = pd.read_csv(O + 'label_queue.csv')
log = pd.read_csv(O + 'labeling_session_log.csv')
wl = pd.read_csv(O + 'sheng_words_list.csv')
inv = json.load(open(os.path.join(HERE, 'upload_inventory.json')))

human = k.existing_label_type == HUM
empty = k.is_empty_after_pii_clean.astype(bool)
ok = k[~empty].copy()
r = ok.date_raw.apply(parse)
ok['year'] = [x[0] for x in r]
ok['form'] = [x[3] for x in r]
ok['era'] = [era(x[0], x[1]) for x in r]
okh = ok[ok.existing_label_type == HUM]
tok = lambda t: re.findall(r"[a-z0-9']+", str(t).lower())

# ---------- section 2: raw counts per source_group ----------
sg = pd.DataFrame({'kept': k.groupby('source_group').size(), 'excluded': e.groupby('source_group').size()}).fillna(0).astype(int)
sg['registry_total'] = sg.kept + sg.excluded
sg['platform'] = k.groupby('source_group').platform.agg(lambda s: '/'.join(sorted(set(s.astype(str)))))
sg['n_files'] = k.groupby('source_group').source_file.nunique()
sg = sg.sort_values('registry_total', ascending=False)
tot = sg[['kept', 'excluded', 'registry_total']].sum()
sg_tbl = sg[['platform', 'n_files', 'registry_total', 'excluded', 'kept']].copy()
sg_tbl.loc['**TOTAL**'] = ['', int(sg.n_files.sum()), int(tot.registry_total), int(tot.excluded), int(tot.kept)]
plat = pd.DataFrame({'kept': k.platform.value_counts(), 'excluded': e.platform.value_counts()}).fillna(0).astype(int)
plat['registry_total'] = plat.kept + plat.excluded
plat = plat.sort_values('registry_total', ascending=False)

# upload schema inventory
sig = collections.defaultdict(list)
for n, rows, cols, enc in inv:
    sig[tuple(cols) if cols else ('(could not be parsed by the default CSV reader)',)].append((n, rows))
sig_rows = []
for cols, items in sorted(sig.items(), key=lambda kv: -len(kv[1])):
    c = '; '.join(x.replace('\ufeff', '').replace('`', "'") for x in cols)
    c = c if len(c) < 140 else c[:137] + '...'
    sig_rows.append({'files': len(items), 'rows (naive parse)': sum(x[1] or 0 for x in items), 'columns': '`' + c.replace('|', '/') + '`',
                     'example file': items[0][0]})
sig_tbl = pd.DataFrame(sig_rows)

# ---------- section 3: exclusions ----------
reas = e.exclusion_reasons.value_counts().rename_axis('exclusion_reasons (as logged)').to_frame('rows')
reas_any = pd.Series(name='rows', data={
    'duplicate (any combination)': int(e.exclusion_reasons.str.contains('duplicate').sum()),
    'punctuation_only (any)': int(e.exclusion_reasons.str.contains('punctuation_only').sum()),
    'truncated_see_more (any)': int(e.exclusion_reasons.str.contains('truncated').sum()),
    'spam_or_promo (any)': int(e.exclusion_reasons.str.contains('spam').sum()),
    'scraper_artifact (any)': int(e.exclusion_reasons.str.contains('scraper').sum()),
}).rename_axis('reason').to_frame()

pe = k.text_plain_emoji.fillna(''); td = k.text_display.fillna(''); tc = k.text_clean.fillna(''); tx = k.text.fillna('')
na = lambda s: any(ord(c) > 127 for c in s)
URL = re.compile(r'https?://\S+|www\.\S+', re.I); MEN = re.compile(r'@\w+'); HASH = re.compile(r'#\w+')
EM = re.compile(r'\b[\w.+-]+@[\w-]+\.[\w.-]+\b'); PH = re.compile(r'(?:\+?254|0)?7\d{8}\b|\b\d{7,12}\b')
c_steps = pd.DataFrame([
    ('text != text_display (URL stripped / stylised fonts mapped / whitespace)', int((tx != td).sum())),
    ('text_display != text_plain_emoji (stray symbols, invisible marks, mojibake dropped)', int((td != pe).sum())),
    ('rows emptied or punctuation-only by the plain+emoji filter', int(k.is_empty_after_filter.sum() + k.is_punct_only_after_filter.sum())),
    ('rows with an @mention in text_plain_emoji', int(pe.str.contains(MEN).sum())),
    ('rows with a #hashtag in text_plain_emoji', int(pe.str.contains(HASH).sum())),
    ('rows with an e-mail address in text_plain_emoji', int(pe.str.contains(EM).sum())),
    ('rows with a phone-like number (7-12 digits) in text_plain_emoji', int(pe.str.contains(PH).sum())),
    ('rows with at least one non-ASCII character (incl. emoji) in text_plain_emoji', int(pe.apply(na).sum())),
    ('rows where text_clean != text_plain_emoji', int((pe != tc).sum())),
    ('rows empty after PII + ASCII clean (kept in master, left out of every export)', int(empty.sum())),
    ('rows flagged possible_leading_name (flag only, NOT removed)', int(k.possible_leading_name.sum())),
    ('residual check: rows in text_clean with non-ASCII / @ / # / URL', f"{int(tc.apply(na).sum())} / {int(tc.str.contains(MEN).sum())} / {int(tc.str.contains(HASH).sum())} / {int(tc.str.contains(URL).sum())}"),
], columns=['check', 'rows'])

# ---------- section 4: labels ----------
h = k[human]
by_src = pd.crosstab(h.source_group, h.existing_label).reindex(columns=['Positive', 'Neutral', 'Negative'])
by_src['Total'] = by_src.sum(axis=1)
by_src = by_src.sort_values('Total', ascending=False)
by_src.loc['**TOTAL**'] = by_src.sum()
tap = log[log.label != 'skip']
tap_c = tap.label.value_counts()
lab_tbl = pd.crosstab(lab.label_source, lab.label).reindex(columns=['Positive', 'Neutral', 'Negative'], fill_value=0)
lab_tbl['Total'] = lab_tbl.sum(axis=1)
lab_tbl.loc['**ALL**'] = lab_tbl.sum()
sd_tbl = pd.crosstab(sd.label_source.fillna('blank'), sd.label.fillna('blank'))
q_stat = q.status.value_counts().rename_axis('status').to_frame('rows')
q_era = q.era.value_counts().sort_index().rename_axis('era').to_frame('queue rows')
q_era['logged (labeled or skipped)'] = log.era.value_counts().reindex(q_era.index).fillna(0).astype(int)

# ---------- section 5: eras ----------
order = ['2020-H1', '2020-H2', '2021-H1', '2021-H2', '2022-H1', '2022-H2', '2022-year-only', '2023-H1', '2023-H2', '2024-H1', '2024-H2',
         '2025-H1', '2025-H2', '2026-H1', '2026-H2', 'undated']
er = pd.DataFrame({'all usable rows': ok.era.value_counts(), 'human-labeled': okh.era.value_counts()}).reindex(order).fillna(0).astype(int)
for c in ['Positive', 'Neutral', 'Negative']:
    er[c] = okh[okh.existing_label == c].era.value_counts().reindex(order).fillna(0).astype(int)
er['binary (Pos+Neg)'] = er.Positive + er.Negative
er.loc['**TOTAL**'] = er.sum()
yr = pd.DataFrame({'all usable rows': ok.year.fillna(0).astype(int).value_counts().sort_index()})
yr.index = ['undated' if i == 0 else i for i in yr.index]
hy = okh.year.fillna(0).astype(int).value_counts().sort_index(); hy.index = ['undated' if i == 0 else i for i in hy.index]
yr['human-labeled'] = hy.reindex(yr.index).fillna(0).astype(int)
yr.loc['**TOTAL**'] = yr.sum()
yr.index = [str(i) for i in yr.index]
yr.index.name = 'year'

emo = k.emoji_sentiment.notna() & ~human
lab_by_year = lab.copy()
lab_by_year['year'] = lab_by_year.date_posted.apply(lambda s: 'undated' if not isinstance(s, str) or not s else s[-4:])
ly = pd.crosstab(lab_by_year.year, lab_by_year.label_source)
ly['Total'] = ly.sum(axis=1)
ly.loc['**TOTAL**'] = ly.sum()

funnel_rows = []
def f(name, d):
    hh = d[d.existing_label_type == HUM]
    funnel_rows.append((name, len(d), int(hh.existing_label.isin(['Positive', 'Negative']).sum()), len(hh)))
f('All rows in cleaned_population.csv', k)
f('...with non-empty text_clean', ok)
f('...with a parseable date', ok[ok.year.notna()])
f('...dated 2023-2026', ok[ok.year.between(2023, 2026)])
f('...dated 2020-2025 (the proposal window)', ok[ok.year.between(2020, 2025)])
f('...containing a Sheng-lexicon term (contains_sheng)', ok[ok.contains_sheng])
f('...Sheng and dated 2023-2026', ok[ok.contains_sheng & ok.year.between(2023, 2026)])
fun = pd.DataFrame(funnel_rows, columns=['subset', 'rows', 'binary human labels (Pos+Neg)', 'all human labels'])

# ---------- section 6: text statistics ----------
def st(s, name):
    s = s.fillna('')
    ch = s.str.len(); wd = s.str.split().str.len()
    return dict(subset=name, rows=len(s), chars_mean=round(ch.mean(), 1), chars_median=int(ch.median()), chars_p90=int(ch.quantile(.9)), chars_max=int(ch.max()),
                words_mean=round(wd.mean(), 1), words_median=int(wd.median()), words_p90=int(wd.quantile(.9)), words_max=int(wd.max()))
hl = lab[lab.label_source == 'human']
stat_rows = [st(ok.text_clean, 'all usable rows (text_clean)'), st(lab.text, 'labeled dataset (human + emoji)'), st(hl.text, 'human-labeled')]
for c in ['Positive', 'Neutral', 'Negative']:
    stat_rows.append(st(hl[hl.label == c].text, f'human {c}'))
stat_rows.append(st(lab[lab.label_source != 'human'].text, 'emoji-heuristic labeled'))
stat_rows.append(st(unl.text, 'sheng_unlabeled_dataset'))
stat_tbl = pd.DataFrame(stat_rows)

def vocab(texts):
    c = collections.Counter()
    for t in texts: c.update(tok(t))
    return c
va, vl, vh, vu = vocab(ok.text_clean), vocab(lab.text), vocab(hl.text), vocab(unl.text)
voc = pd.DataFrame([
    dict(subset=n, tokens=sum(v.values()), types=len(v), hapax=sum(1 for x in v.values() if x == 1), type_token_ratio=round(len(v) / sum(v.values()), 3))
    for n, v in [('all usable rows', va), ('labeled dataset (human + emoji)', vl), ('human-labeled only', vh), ('sheng_unlabeled_dataset', vu)]])
topw = pd.DataFrame(vl.most_common(20), columns=['token (labeled dataset)', 'count'])

# lexicon provenance
m = wl.documented_meaning.fillna('')
cat = collections.Counter()
for x in m:
    if '(user-confirmed)' in x: cat['user-confirmed meaning'] += 1
    elif 'no meaning recorded' in x: cat['user-added, no meaning recorded'] += 1
    elif x.startswith('not verified'): cat['sheng.co.ke Kamusi (T-section), meaning not verified'] += 1
    elif 'web-sourced' in x: cat['web-sourced meaning'] += 1
    elif x.strip() == '': cat['blank'] += 1
    else: cat['other (glossary-derived or earlier batches, provenance not itemised)'] += 1
lex_tbl = pd.DataFrame(sorted(cat.items(), key=lambda kv: -kv[1]), columns=['meaning provenance', 'terms'])
lex_tbl.loc[len(lex_tbl)] = ['**TOTAL**', len(wl)]
topterms = wl.sort_values('rows_in_corpus_matching', ascending=False).head(12)[['term', 'rows_in_corpus_matching']]

def midcap(t):
    # True if a capitalised word appears mid-sentence (possible personal or place name); examples avoid these
    w = str(t).split()
    return any(re.match(r"^[A-Z][a-z]+$", x) and not re.search(r'[.!?:]$', w[i - 1]) for i, x in enumerate(w) if i > 0)


# examples (anonymised: only rows with no leading-name flag and no mid-sentence capitalised word; text_clean only, no author column)
name_ok = set(k[~k.possible_leading_name].row_id)
ex_rows = []
for c, n in [('Positive', 3), ('Neutral', 3), ('Negative', 3)]:
    pool = lab[(lab.label == c) & (lab.label_source == 'human') & lab.id.isin(name_ok) & lab.text.str.len().between(35, 140) & ~lab.text.apply(midcap)]
    for t in pool.sample(n, random_state=11).itertuples():
        ex_rows.append((c, 'human', t.id, t.date_posted if isinstance(t.date_posted, str) else '', t.text))
for c in ['Positive', 'Negative']:
    pool = lab[(lab.label == c) & (lab.label_source != 'human') & lab.id.isin(name_ok) & lab.text.str.len().between(35, 140) & ~lab.text.apply(midcap)]
    for t in pool.sample(2, random_state=11).itertuples():
        ex_rows.append((c, 'emoji heuristic', t.id, t.date_posted if isinstance(t.date_posted, str) else '', t.text))
ex_tbl = pd.DataFrame(ex_rows, columns=['label', 'source', 'id', 'date_posted', 'text'])
ex_tbl['text'] = ex_tbl.text.str.replace('|', '/', regex=False)
unl_ex = unl[unl.id.isin(name_ok) & unl.text.str.len().between(35, 140) & ~unl.text.apply(midcap)].sample(4, random_state=11)[['id', 'date_posted', 'text', 'sheng_words_used', 'sheng_meanings']].fillna('')
unl_ex['text'] = unl_ex.text.str.replace('|', '/', regex=False)

# ---------- file facts ----------
def finfo(path, df=None):
    d = df if df is not None else pd.read_csv(path)
    return len(d), len(d.columns)

files = pd.DataFrame([
    ('cleaned_population.csv', *finfo(None, k), 'Master table: every kept row, all text versions, all label columns, Sheng flag'),
    ('cleaned_population_sheng_only.csv', *finfo(None, so), 'Rows of the master whose text matches the Sheng lexicon (current labels)'),
    ('excluded_rows_log.csv', *finfo(None, e), 'Every row removed in cleaning, with exclusion_reasons'),
    ('sheng_labeled_dataset.csv', *finfo(None, lab), 'Delivered: all labeled rows (human + emoji heuristic), cleaned text, Sheng words and meanings'),
    ('sheng_unlabeled_dataset.csv', *finfo(None, unl), 'Delivered: Sheng rows with no sentiment label of any kind, label column blank'),
    ('sentiment_dataset.csv', *finfo(None, sd), 'STALE snapshot of the earlier export (built before most of the lexicon growth and tap labeling)'),
    ('labeling_workbook.xlsx', 21160, 9, 'STALE annotation workbook (Labeling + Codebook sheets); same snapshot as sentiment_dataset.csv'),
    ('sheng_words_list.csv', *finfo(None, wl), 'Lexicon with documented meaning and corpus match count'),
    ('label_queue.csv', *finfo(None, q), 'Tap-labeling queue (row_id, era, text_clean, status)'),
    ('labeling_session_log.csv', *finfo(None, log), 'Audit log of tap-labeling answers (label or skip, era, UTC time)'),
    ('records_added_today_2026-09-28.csv', 5082, 12, 'The 5,082 rows added in the 2026-09-28 update (as reported in cleaning_report.md)'),
    ('records_added_today_sheng_only_2026-09-28.csv', 1291, 13, 'Sheng-matching subset of that update'),
    ('cleaning_report.md', '', '', 'Earlier cleaning report; figures in it are from an earlier state of the data'),
], columns=['file', 'rows', 'columns', 'purpose'])


n_fbdata = sum(1 for n, *_ in inv if n.startswith('facebook_data_'))


FENCE = '`' * 5   # built here so this file never contains a literal 5-backtick line that could close its own embedded copy


def read(p):
    return open(os.path.join(HERE, p)).read()

snip = json.load(open(os.path.join(HERE, 'snippets.json')))
today = datetime.date.today().isoformat()

# =====================================================================================
doc = f"""# Data work record: Sheng sentiment corpus (MSc thesis, JKUAT)

*Adaptive NLP for Rapidly Evolving Languages: A Continual Learning Approach with Application to Sheng* (proposal V20260706-2.1).
Record of the data work done in this chat, written {today}. All data files are in the chat's outputs folder.

## 0. How to read this record

Every figure carries one of three confidence tags, because part of this chat's history was lost when the working environment was reset.

| Tag | Meaning |
|---|---|
| **[V]** | Verified: recomputed from the files on {today}, by the code in the appendix. |
| **[S]** | Reported earlier in the chat (earlier messages, `cleaning_report.md`, or the summary written when the chat's context was compacted). The underlying files or scripts no longer exist, so I could not re-check it. |
| **[?]** | Not known or not recorded anywhere I can reach. Left open on purpose. Do not fill these with estimates. |

What was lost, and what it means:

* The original pipeline scripts (`build_master`, `clean_master`, `compute_exclusions`, `finalize`, `clean_pii_ascii`, `emoji_sentiment`, the id registry and the original human-label file) were wiped by sandbox resets. **Their source code cannot be reproduced.** Section 8 lists them as "not recoverable" and gives the regular expressions and the descriptions that survive. It does not invent code.
* The saved transcript on disk starts at the compaction on 2026-10-08, so everything before that date is known only from the summary and from the files themselves.
* Counts in sections 2 to 6 come from the output files, which survived. Where a step's count cannot be derived from the files, it is tagged [S] or [?].

## 1. Data sources

### 1.1 What the corpus is made of [V for counts, S for descriptions]

The corpus is 25,886 records from 24 source groups across Twitter/X, Facebook, TikTok, Instagram and curated or pasted Sheng collections. Per-source record counts are in section 2. All of it reached this chat as **files you uploaded or text you pasted**; nothing was scraped or fetched by me. The one use of the web was to look up Sheng vocabulary for the lexicon (section 3.8), which is not corpus text.

| Source group (as named in the data) | Platform | What it is | How it was collected | Date evidence |
|---|---|---|---|---|
| `twitter_scrape` | Twitter/X | 49 export files, `twitter_data_batch_*` and `twitter_data_2026-09-0*` (schema: date, author, post, empty `sentiment`) | Scraper exports, uploaded by you. Tool and method [?]. | Dates in the rows; file names carry 2026-09-08 and 2026-09-09 |
| `zeeschuimer_twitter` | Twitter/X | Zeeschuimer browser-extension export | Browser capture while browsing, by you. [S] from the tool's name and schema | File name `2026-08-23` |
| `zeeschuimer_tiktok` | TikTok | Zeeschuimer export (36 columns incl. `body`, `hashtags`, play counts) | Browser capture [S] | File name `2026-08-31` |
| `zeeschuimer_instagram` | Instagram | Zeeschuimer export; its CSV quoting was corrupted, so only 11 captions were recovered [S] | Browser capture [S] | File name `2026-08-31` |
| `twitter_sheng_flag` | Twitter/X | `twitter_data_with_sheng_flag.csv` (columns `post_date, author, post_text, has_sheng, sheng_terms_str`) | Prior pipeline output with a Sheng flag, uploaded by you; origin [?] | Dates in the rows |
| `facebook_with_dates`, `facebook_manual`, `facebook_generic` | Facebook | `facebook_posts_with_dates*.csv`, `facebook_posts_manual*.csv` and {n_fbdata} `facebook_data_<mm-yy>_batch_<n>_2026-09-09T*.csv` files (columns `Date, Author, Post/Comment, Parent Post, Type, Hashtags`) | Scraper and manual copy exports, uploaded by you. Tool and method [?] | File names carry `2026-09-09`; row dates vary |
| `Matwana_2020`, `Matwana_2021`, `Matwana_2022` | Facebook | Posts and comments from the Matwana source, one group per year | Pasted or uploaded by you [S]; method [?] | Year in the group name |
| `Kartelo`, `bongasheng` | Facebook | Comment sets from two Facebook pages | Method [?] | Row dates |
| `sheng_social_media_dataset`, `sheng_social_media_dataset_2` | Unspecified social | Curated Sheng posts with a `sentiments(PG or None PG)` column (a content rating, not sentiment) | Uploaded by you; original compiler [?] | Row dates |
| `sheng_summary_composite`, `sheng_summary_no_urls` | Unspecified social | Summary tables with columns for Sheng used and meaning | Uploaded by you; compiler [?] | Row dates |
| `sheng_news_posts` | News / Facebook | News-page posts, columns include `Sheng Used`, `Meaning`, `Label` | Uploaded by you | Row dates (e.g. `4-Jul-24`) |
| `sheng_mtaa_twitter` | Twitter/X | Posts from a "Sheng Mtaa on X" slang-of-the-day account; almost entirely glossary-annotated [S] | Pasted by you [S] | Row dates (month-first with time) |
| `sheng_manual_batch2_*` (`sheng_news`, `Ghetto_Radio_Sheng_News`, `Lughayangu`, `shengtezzo`, `Facebook_Matwana`) | Unspecified social | A second manual paste of five sources (497 rows in the paste [S]) | Pasted by you [S] | Row dates |

Dates here mean what the files themselves say (file-name timestamps or row dates). **I have no record of the dates you actually ran the collections**, only of when files reached the chat. [?]

### 1.2 Permissions and ethics

What the proposal commits to (Section 3.6.2 and 3.8 of `SESERA - THESIS PROPOSAL V20260706-2.1`) [V, read from the docx]:

* Collect public posts from Twitter/X, Facebook, Reddit and forums through API access and ethical web scraping.
* Restrict collection to public-facing content where users have no reasonable expectation of privacy.
* Remove all personally identifiable information using rule-based systems and a fine-tuned NER model.
* Submit the collection and handling protocol to the **JKUAT Ethics Review Committee for formal approval before any data collection begins**.
* Store data encrypted with access restricted to the research team; scripts identify themselves with a user agent and researcher contact.
* Adhere to platform terms of service (the proposal names the Twitter Developer Agreement and the Reddit API terms).

What this chat shows against those commitments:

* **Ethics approval: [?]** No approval letter, reference number or date appears anywhere in this chat. I cannot say whether it was obtained, or whether it was obtained before the files dated 2026-08 and 2026-09 were collected. Please add this from your own records.
* **Platform terms: not verified.** I do not know how the Twitter/X, Facebook and TikTok exports were collected. In this chat I advised that scraping Facebook, X and TikTok can breach platform terms and may engage Kenya's Data Protection Act 2019, and listed access routes that avoid that (Meta Content Library, the YouTube Researcher Program where eligible, consent-based collection, written agreements with content owners). That was general guidance and not legal advice. Whether the corpus complies is an open question for you and your supervisors.
* **Reddit, forums and blogs:** named in the proposal but **absent from the corpus**. No Reddit data was provided.
* **Anonymisation actually done:** section 3.7. It is rule-based only. **No NER model was run**, and names inside post text were flagged but not removed.
* **Storage and access:** nothing in this chat shows encryption or access control. The output CSVs sit unencrypted in the chat's outputs folder. [V] The master file still contains an `author` column (18,621 non-empty values), which is identifying; the two delivered datasets leave it out.

## 2. Raw data

### 2.1 Records per source before cleaning [V]

"Registry total" is every record that ever received a stable row id (`R000001` onward) = kept rows + rows in `excluded_rows_log.csv`. The registry is **additive**: when a later upload overwrote an earlier file of the same name, the earlier rows were kept, so these totals include versions of files that no longer exist as uploads. Because of this, the registry total cannot be reproduced from the upload folder alone.

{md(sg_tbl)}

`n_files` counts distinct `source_file` names in the registry (including earlier versions of overwritten files), so its total ({int(sg.n_files.sum())}) differs from the number of files currently in the upload folder.

By platform:

{md(plat)}

The registry total is **{int(tot.registry_total):,}** records, of which **{int(tot.kept):,}** were kept and **{int(tot.excluded):,}** excluded. [V]

Earlier milestones, reported at the time [S]:

* 84 raw CSV files, 7 distinct schemas, **20,034 raw rows ingested** at the first cleaning (`cleaning_report.md`). The report does not say whether this figure is before or after the four whole-file drops in 3.1. [?]
* 5,082 rows were added in the 2026-09-28 update (nine new Twitter batch files plus changed ones), taking the registry to 25,886. The rows are in `records_added_today_2026-09-28.csv`. [S; the file is present with 5,082 rows [V]]
* About 770 further rows came from pasted sources (273 `sheng_mtaa_twitter` and 497 across five sources). [S]

### 2.2 Formats of the raw upload files [V]

The upload folder in this session holds {len(inv)} CSV files (plus the proposal `.docx` and the labelled `.xlsx`). A default CSV reader parses them into {sum(r or 0 for _, r, _, _ in inv):,} rows in total. This is **not** the registry total, because several files are duplicates or overwritten versions, and two files (a Zeeschuimer Twitter export and `sheng_summary_composite.csv`) have encoding or quoting faults that the default reader cannot parse.

{md(sig_tbl, index=False)}

Delimiters differ: some files are comma-separated and some semicolon-separated, which is why the same schema appears twice with different column strings. The Twitter files have a `sentiment` column, and every value in it is empty. [S, `cleaning_report.md`: "0 of 10,691 rows filled"]

## 3. Cleaning and preprocessing

### 3.1 Steps in the order they were done [S unless marked]

1. **Parse seven schemas into one** (`row_id, source_group, platform, source_file, author, date_raw, text, ...`). [S]
2. **Drop four redundant files in full** because a larger file already contained them: `facebook_posts_with_dates.csv` (347 rows, all in `__1_`), `facebook_posts_with_dates__2_.csv` (51 rows, all in `__3_`), `sheng_social_media_dataset__1_.csv` (2,310 rows, byte-identical to the original), `twitter_data_sheng_only.csv` (95 rows, all in `twitter_data_with_sheng_flag.csv`). That is 2,803 rows. [S]
3. **Assign permanent row ids** through a registry so labels keyed to an id never move when rows are added. [S] The registry file is lost. [V]
4. **Row-level exclusion**: flag a row for one or more reasons, remove it from the master and write it to `excluded_rows_log.csv` (below).
5. **Stylised-font normalisation**: mathematical-alphabet characters mapped back to plain Latin in `text_display`; the raw string is kept in `text`. Reported as 88 rows at the first cleaning [S]; 8 rows carry the flag in the current master [V]. I did not record why the count dropped.
6. **URL stripping** from `text_display` (raw kept in `text`). [S]
7. **Plain-text and emoji filter** to make `text_plain_emoji`: keep letters, digits, punctuation, whitespace and emoji; drop stray symbols, invisible marks and mojibake bytes. No row became empty or punctuation-only. [V counts below]
8. **Emoji heuristic** to make `emoji_sentiment` (section 4.7).
9. **PII and ASCII clean** to make `text_clean` (3.7).
10. **Sheng lexicon flag** to make `contains_sheng` and `matched_sheng_terms` (3.8). This is re-run every time the lexicon changes.

The order of the exclusion rules inside step 4 is **not recorded** [?]. Because one row can carry several reasons, a sequential "records removed per step" funnel would be invented. What I can give exactly is the count per reason and the overlaps.

### 3.2 Row-level exclusions, per logged reason combination [V]

{md(reas)}

Per reason, counting a row once in each reason it carries (a row with two reasons appears twice, so these sum to more than {len(e):,}):

{md(reas_any)}

These match the totals reported in `cleaning_report.md` (4,319 / 265 / 125 / 43 / 22). [V]

* **Duplicate**: second and later occurrence of identical text after Unicode, whitespace and case normalisation. The first occurrence is kept and its `duplicate_count` records how many copies existed. The dedup was fixed so that a labeled row is never dropped in favour of an earlier unlabeled copy. [S] {int((k.duplicate_count > 1).sum()):,} kept rows have `duplicate_count` above 1. [V]
* **Truncated**: text ending in a client-side "... See more". [S]
* **Punctuation or digits only**: for example `,,,,` or `2100`. Emoji-only comments were deliberately kept. [S]
* **Spam or promo**: bare links or "subscribe / click here" posts. 1,502 other rows containing a URL were kept. [S]
* **Scraper artifact**: the literal template rows "Physics will prove you wrong" and ",,,," that appeared as the first rows of 9 or more unrelated files. [S]

Result: {int(tot.registry_total):,} registry rows - {int(tot.excluded):,} excluded = **{int(tot.kept):,} kept** in `cleaned_population.csv`. [V]

### 3.3 Text-level steps on the {len(k):,} kept rows [V]

{md(c_steps, index=False)}

### 3.4 Normalisation [S]

Stylised fonts mapped to Latin, URLs removed from the display text, stray symbols and mojibake removed. The proposal's normalisation items "colloquial spellings, elongated words, emoji conversion" were, as far as I can tell, **not** done (no such step was run in this chat): spelling variants and elongations are left as written, and emoji are deleted from `text_clean` and are not converted to text.

### 3.5 Accent handling [V]

`text_clean` is ASCII only (0 non-ASCII characters remain). Accented letters were **deleted**, not transliterated, so a word such as one containing `ê` loses that letter. I offered transliteration (ê to e) and the offer was never answered.

### 3.6 Language detection [V]

**None was run.** There is no language-ID step. "Sheng" is decided only by the lexicon match in 3.8, so a row flagged as Sheng is one that contains a listed term and is not verified as Sheng by any language-ID model or annotator.

### 3.7 Anonymisation (PII) [V for counts, S for code]

Removed from `text_clean`, in this order (regular expressions recovered from the saved summary; the script itself is lost):

1. URLs (`https?://\\S+|www\\.\\S+`).
2. E-mail addresses.
3. @mentions (`@\\w+`).
4. #hashtags (`#\\w+`).
5. Phone-like numbers: Kenyan mobile pattern `(?:\\+?254|0)?7\\d{{8}}` or any 7 to 12 digit run.
6. All non-ASCII characters (this deletes every emoji), then whitespace collapsed.

Row counts for each of these are in the table in 3.3. Limits:

* **Names written in plain text are not removed.** The proposal promises a fine-tuned NER model; none was built or run. Rows where the text starts with something name-like (two to four capitalised words) are only **flagged** (`possible_leading_name`, {int(k.possible_leading_name.sum()):,} rows), and the examples in section 6 use only unflagged rows.
* The phone pattern also removes any long digit string (prices, ids). Numbers such as "Ksh 1500" are kept; seven-digit-plus numbers are not.
* The master still holds the author handle or name in `author`. Anonymisation is applied to the text columns and to the two delivered datasets, which exclude `author`. The master file should be treated as sensitive.
* The `author_hash` approach I recommended in the schema advice was **not implemented**.

### 3.8 Sheng lexicon and the Sheng flag

The Sheng flag is a **whole-word, case-insensitive regular-expression match** of lexicon terms against `text_plain_emoji`, longest term first. The word `form` does not match when followed by `1`-`4` or `one`-`four` (to avoid "form 3", a school year). Code is in the appendix (`filter_sheng_only.py`).

Lexicon size now: **{len(wl)} terms** [V]. Where the terms came from [V counts, S for attribution]:

{md(lex_tbl, index=False)}

* The base lexicon was built from glossary-style rows inside the corpus plus web sweeps (sheng.co.ke, and other sites I searched, whose pages I read as web results), and was extended by your messages in the form `word=meaning`. User-confirmed meanings are marked `(user-confirmed)` in `sheng_words_list.csv`.
* {int((wl.rows_in_corpus_matching == 0).sum())} of {len(wl)} terms match no row in the corpus. [V]
* **Risky words kept at your instruction** because they also occur as ordinary English or Swahili: `time`, `so`, `k`, `red`, `tano`, `toka`, `kudade`, `dre`, `chacha`, `jake`, `kutu`, `dumbo`, `mongo`, `form`, `sonko`, `chai`, `scene`, `blue`, `bob`. [S] I recommended removing `so`, `k` and `red` from the active filter and you did not answer, so they are still active.
* The most frequent matched terms (rows matched): {', '.join(f"`{t}` {n}" for t, n in zip(topterms.term, topterms.rows_in_corpus_matching))}. [V] `so` and `time` are among the top two, which shows how much the flag over-matches ordinary English.
* Result: **{int(k.contains_sheng.sum()):,} of {len(k):,} rows ({k.contains_sheng.mean() * 100:.1f}%) are flagged as containing Sheng** [V], held in `cleaned_population_sheng_only.csv` ({len(so):,} rows).

### 3.9 Exports built from the master

* `sheng_labeled_dataset.csv` and `sheng_unlabeled_dataset.csv` were built from the current master (section 5).
* `sentiment_dataset.csv` and `labeling_workbook.xlsx` are **stale** (21,160 rows each, built before the lexicon grew and before the tap-labeling session). Do not use them for counts. Rebuild them from the master if you want them current.

## 4. Labelling

### 4.1 Label set [V]

Three classes are stored: **Positive, Neutral, Negative**. The proposal specifies **binary (positive / negative)** (Section 3.6.2). The mismatch is unresolved: either the proposal needs an amendment to three classes, or Neutral rows are set aside for the binary experiments. In the data, human labels are {int((hl.label == 'Positive').sum()):,} Positive, {int((hl.label == 'Neutral').sum()):,} Neutral and {int((hl.label == 'Negative').sum()):,} Negative. [V]

### 4.2 What each label means, and guidelines [?]

* The only written definition anywhere is the codebook sheet in `labeling_workbook.xlsx`: `sentiment_label = positive / negative / neutral`, and `label_source = human = your manual label; emoji_heuristic = inferred from emoji; blank = unlabeled`. [V]
* **No annotation guideline (decision rules for sarcasm, code-switching, quotes, news headlines, questions, and so on) was written or shared in this chat.** The proposal calls for "a standardised annotation guideline"; it does not exist here. [?]
* In the tap-labeling interface the only options were Positive, Neutral, Negative and Skip. A Skip means "do not label" and is not a class. [V]

### 4.3 Who labelled, and how many labellers [?]

* All human labels in this chat were entered or supplied by **you**: a large pasted batch, a labelled spreadsheet you uploaded, and the tap-labeling sessions. I did not label any row and no AI-produced sentiment labels exist in the data. [S, consistent with the label columns [V]]
* **The number of labellers is not recorded.** The files do not say whether the pasted batch and spreadsheet were produced by one person or several, and the proposal calls for "native Sheng speakers" (plural). Fill this in from your own records. [?]
* The tap-labeling answers (106 labels) are in `labeling_session_log.csv` with a UTC timestamp, from the `{log.time.min()[:10]}` to `{log.time.max()[:10]}` sessions. [V]

### 4.4 Provenance of the {int(human.sum()):,} human labels [V]

| Stage | Human labels | Evidence |
|---|---|---|
| Before the tap-labeling sessions | **{int(human.sum()) - len(tap):,}** | Derived: {int(human.sum()):,} total - {len(tap)} tap labels. Matches the 2,002 reported when the pasted batch and the spreadsheet were merged [S], and the 733 + 533 + 736 = 2,002 human rows in the stale `sentiment_dataset.csv` [V]. |
| Tap-labeling sessions | **{len(tap)}** | `labeling_session_log.csv`: {int(tap_c.get('positive', 0))} Positive, {int(tap_c.get('neutral', 0))} Neutral, {int(tap_c.get('negative', 0))} Negative; plus {int((log.label == 'skip').sum())} skipped |
| **Total** | **{int(human.sum()):,}** | `existing_label_type == manual_sentiment_label (Positive/Negative/Neutral)` in `cleaned_population.csv` |

How the earlier labels arrived [S]:

* An initial 56 human labels existed on `sheng news` rows (54 survived cleaning).
* A batch of 1,875 hand-labelled rows (676 Negative, 697 Positive, 502 Neutral) was merged by `row_id`. Checks before merging: every id existed, none repeated, none conflicted, and 46 of 47 rows that already carried a label agreed. The one disagreement was `R020386` (earlier Negative, new Positive); the new label was taken.
* A second paste had text corruption (mojibake), and 43 of 354 rows would not match, so you uploaded a spreadsheet and the merge was redone from it by id (reported total 2,002). One conflict was found, `R000067` (Negative, then Positive); I applied the newer value and told you.
* Codes `pg` and `d` in the spreadsheet were **never applied** as sentiment. You have since said that `pg` means the content is not appropriate for children and `none pg` means it is appropriate. That is a content rating, not sentiment.

**Disagreement resolution: there was no multi-annotator disagreement process.** Where a row received two different labels from the same source (the two cases above), the later value replaced the earlier one. [S]

### 4.5 Inter-annotator agreement [V]

**None was measured.** No row was labelled by two annotators in this chat, so there is no Cohen's kappa or any other agreement score. The proposal targets kappa of at least 0.80 and requires it. Section 7 lists this as the biggest methodological gap.

### 4.6 Tap-labeling sample design [S for design, V for counts]

The queue of {len(q):,} rows was selected from natural (non-glossary), unlabeled, Sheng-flagged rows of 30 to 220 characters and at least 5 words, in the eras 2023-H2 through 2026-H1, de-duplicated on normalised text, and interleaved round-robin across eras with random seed 7. The queue's own contents: {', '.join(f'{a} {b}' for a, b in q.era.value_counts().sort_index().items())}. Status now:

{md(q_stat)}

Because of the round-robin, the 126 logged answers are balanced across eras (21 per era). [V]

### 4.7 Emoji-heuristic labels (not human) [V counts, S for rule]

* The master has {int(k.emoji_sentiment.notna().sum())} rows with an `emoji_sentiment` value: {int((k.emoji_sentiment == 'Positive').sum())} Positive and {int((k.emoji_sentiment == 'Negative').sum())} Negative; it never assigns Neutral.
* {int((k.emoji_sentiment.notna() & human).sum())} of them also have a human label; the human label takes priority, which leaves **{int(emo.sum())}** emoji-only labels.
* **The rule (which emoji count as positive or negative, and how ties are handled) is not recoverable.** The script is lost and I did not record the emoji lists. [?]
* These are weak, unverified labels. They are 90% Positive. Use them for pre-training or analysis if at all, and not as ground truth for evaluation.

## 5. Final dataset

### 5.1 Files produced [V]

{md(files, index=False)}

### 5.2 Columns

`sheng_labeled_dataset.csv`: `id`, `date_posted`, `text`, `contains_sheng`, `sheng_words_used`, `sheng_meanings`, `label`, `label_source`.

`sheng_unlabeled_dataset.csv`: `id`, `date_posted`, `text`, `sheng_words_used`, `sheng_meanings`, `label` (blank).

* `id` is the permanent `R######` row id. `date_posted` is `dd/mm/yyyy`, `mm/yyyy` or `yyyy`, converted from about 20 raw formats (rules in `datefmt.py`). `text` is `text_clean` (ASCII, PII removed). `sheng_meanings` lists `term = meaning` only for terms with a recorded meaning (placeholder and "not verified" entries are treated as no meaning).
* In `sheng_labeled_dataset.csv`, `label_source` is `human` or `emoji_heuristic (unverified)`.
* **Dates**: the raw formats mix day-first (`dd/mm/yyyy`, 8,895 rows with a first part above 12 and none with a second part above 12 when this was checked [S]), ISO year-first, month-first with time (`sheng_mtaa_twitter` only), `d-Mon-yy`, month names, `mm/yy`, and bare years. A year-only date (594 rows, almost all `Matwana_2022`) cannot be placed in a half-year. Many Facebook sources are dated 2026 and **may be the scrape or export date rather than the post date**, so 2026 counts may be inflated. `02/05` (9 rows from `facebook_generic`) and `0` (12 rows) cannot be read as a date at all and are left blank, giving {int(lab.date_posted.isna().sum())} blank dates in `sheng_labeled_dataset.csv` and {int(unl.date_posted.isna().sum())} in `sheng_unlabeled_dataset.csv`.
* **Date-converter fix, 2026-11-25.** The first versions of both datasets left 11 `Lughayangu` rows with dates like `January 15, 2023` blank (9 of them labeled rows). I added that format to the converter and rebuilt `sheng_labeled_dataset.csv`; `sheng_unlabeled_dataset.csv` was rebuilt too and did not change. Download the labeled file again if you saved the earlier copy.

### 5.3 Row counts and class counts [V]

`sheng_labeled_dataset.csv` ({len(lab):,} rows):

{md(lab_tbl)}

* Rows that contain a Sheng term: {int((lab.contains_sheng == 'yes').sum()):,}; rows that do not: {int((lab.contains_sheng == 'no').sum()):,}.
* Human-labeled rows that contain Sheng: {int((human & k.contains_sheng).sum()):,} of {int(human.sum()):,}.
* Binary (Positive + Negative, human only): **{int(h.existing_label.isin(['Positive', 'Negative']).sum()):,}** rows, which is about {h.existing_label.isin(['Positive', 'Negative']).sum() / 12000 * 100:.0f}% of the proposal's ~12,000 labelled-example target.

`sheng_unlabeled_dataset.csv`: **{len(unl):,}** rows, `label` blank, with every row carrying at least one Sheng term. It excludes every human-labeled row, every emoji-labeled row and any row left empty after cleaning. [V]

### 5.4 Train / validation / test splits [V]

**No split has been made.** The proposal plans a per-era stratified 70 / 15 / 15 split with six half-year eras from 2020 to 2025 and about 2,000 annotated samples per era. None of that exists yet: there is no `split` column in any file.

### 5.5 Era (time) distribution [V]

The proposal's six eras (2020 to 2025, half-year) cannot be filled from this data. Rows by era, all usable rows versus human-labeled rows (the era uses the date parser in `datefmt.py`):

{md(er)}

By calendar year:

{md(yr)}

* The 2022 rows are mostly year-only (594 of 689), so 2022 cannot be split into halves.
* 2020 to 2022 together have only {int(okh[okh.year.between(2020, 2022)].shape[0])} human labels.
* The proposal's window ends in 2025, while {int((ok.year == 2026).sum()):,} of {len(ok):,} usable rows are dated 2026. Options I put to you, unanswered: shift the window to 2023 to 2026, merge 2020 to 2022 into one legacy era, or use data-driven eras.

All labeled rows (human + emoji) by year, as they appear in `sheng_labeled_dataset.csv`:

{md(ly)}

Usable-data funnel (rows and human labels at each filter) [V]:

{md(fun, index=False)}

Earlier in the chat I also reported rows after "excluding glossary and definition-style rows" (19,523 rows, 977 binary labels) and the Sheng subset of those (4,835 rows, 495 binary labels). The glossary rule was in a lost script, so these are **[S] and not reproduced here**.

## 6. Statistics and examples

### 6.1 Text length [V]

Characters and whitespace-separated words, measured on `text_clean`:

{md(stat_tbl, index=False)}

The longest text is {int(ok.text_clean.str.len().max())} characters. That ceiling suggests a source-side length cap; I did not check which sources set it.

### 6.2 Vocabulary [V]

Tokens are lower-cased runs of letters, digits and apostrophes. There is no stemming, spelling normalisation or stop-word removal.

{md(voc, index=False)}

The 20 most frequent tokens in the labeled dataset:

{md(topw, index=False)}

The list is dominated by Swahili and English function words (`na`, `ni`, `ya`, `kwa`, `the`, `to`), and `rt` appears {vl.get('rt', 0)} times, which shows retweet markers left in the text.

### 6.3 Examples per class [V]

These come from `sheng_labeled_dataset.csv`, drawn at random (seed 11) from rows with no name-like leading text and no capitalised word inside a sentence (to avoid personal names). They carry the cleaned text only, with no author. A name inside a sentence could still slip through, so check each before quoting it in the thesis.

{md(ex_tbl, index=False)}

Unlabeled examples from `sheng_unlabeled_dataset.csv`:

{md(unl_ex, index=False)}

## 7. Decisions and limitations

### 7.1 Decisions made, and why

| Decision | Why | Source |
|---|---|---|
| Keep three classes | You labelled Positive / Neutral / Negative from the start | S |
| Never use `pg` / `none pg` as sentiment | It is a content-appropriateness rating (you confirmed: `pg` = not appropriate for children) | S, V |
| Human label overrides the emoji label | Human labels are verified, emoji labels are not | V |
| Keep the first occurrence of duplicate text and count the copies | Avoids duplicated training or test rows while keeping a frequency signal | S |
| Dedup never drops a labeled row in favour of an unlabeled copy | A labeled row is more valuable than an earlier unlabeled twin | S |
| Keep emoji-only comments in the master | They carry sentiment signal | S |
| Strip emoji, hashtags, mentions, URLs, phone numbers and non-ASCII from `text_clean` | You asked for emoji, hashtags, PII and non-ASCII removal; this is the annotator-facing and export text | S |
| Keep risky lexicon words active | Your instruction | S |
| Sheng flag by lexicon match | No language ID exists for Sheng | V |
| Day-first reading of `dd/mm/yyyy`; month-first only for the `m/d/yyyy hh:mm` source | Tested on the data (no second part above 12) | S |
| Stable row ids and an additive registry | Labels must survive new uploads | S |
| Leave ambiguous dates blank (`02/05`, `0`) | Cannot be placed safely | V |
| Treat meaning placeholders and "not verified" as no meaning | They are not meanings | V |
| Tap-labeling queue: natural text only, eras round-robin | Balance across eras and avoid glossary rows | S |

### 7.2 Known problems

* **Too few labels.** {int(h.existing_label.isin(['Positive', 'Negative']).sum()):,} human binary labels against the proposal's ~12,000. Only {int(okh[okh.year.between(2023, 2026)].shape[0]):,} human labels fall in 2023 to 2026, and fewer than 100 fall in some single half-years.
* **Era design unworkable as written.** 2020 to 2022 are almost empty, 2026 dominates, and 2022 is mostly year-only. 2026 dates may be scrape dates.
* **Class imbalance.** Human: Positive {int((hl.label == 'Positive').sum())}, Neutral {int((hl.label == 'Neutral').sum())}, Negative {int((hl.label == 'Negative').sum())} (fairly even). Emoji labels: 842 Positive to 86 Negative, no Neutral, so mixing them skews the data strongly Positive.
* **Label provenance is uneven.** Human labels come mostly from the curated Sheng datasets (714), `twitter_scrape` (352) and `Matwana_2022` (143), and only {int((h.platform == 'twitter').sum())} of the {int((k.platform == 'twitter').sum()):,} Twitter/X rows. A model trained on them is not trained on a random sample of the corpus.
* **No agreement score and no guideline.** Label quality is unmeasured. Short texts, sarcasm and code-switching make single-annotator noise likely. Neutral is the shortest class (median 43 characters).
* **Sheng flag is noisy.** Ordinary words (`so`, `time`) inflate it; rows that match only on these are not necessarily Sheng. Misses are also likely: variants and inflections of lexicon words (`mashashola`, `masenke`, `tembea`, `mabei`) and Shembeteng forms (syllable-inserted slang) are not matched. A pattern rule for Shembeteng was offered and not built.
* **Text noise.** Retweet markers (`rt`), news-headline and promotional rows, and glossary-style rows remain in the data, and many rows are mostly English or Swahili with little Sheng (the share was not measured). Accented letters were deleted, and deleting emoji removes sentiment cues from `text_clean`.
* **Anonymisation is partial** (section 3.7). The master file contains author handles.
* **Provenance and compliance unknown** (section 1.2): ethics approval, scraping method and platform terms are not documented here.
* **Truncated and corrupted sources.** 125 truncated rows were removed. The Instagram export recovered only 11 captions and may still hold merged or truncated text.
* **Meanings are uneven.** {int(cat['sheng.co.ke Kamusi (T-section), meaning not verified'])} lexicon entries came from a dictionary page without checked meanings, and 51 have no meaning. `kudade` was recorded as "to cum" but the corpus use reads like an adjective meaning great; I asked and you never answered. `rotejo`, `sagara`, `tishoo` still have no confirmed meaning. [S]
* **Lost reproducibility.** The original pipeline cannot be re-run from scripts (section 8.2). Treat `cleaned_population.csv` as the source of truth, and archive it.

### 7.3 Open decisions waiting for you

1. Three classes versus binary (amend the proposal or drop Neutral).
2. The era window (2023 to 2026, a legacy era, or data-driven eras).
3. Whether to remove `so`, `k` and `red` from the Sheng filter.
4. Whether to transliterate accents instead of deleting them.
5. Whether to convert the data to the schema I proposed (instances / labels / weak labels / splits / lexicon / exclusions, with `author_hash`, Parquet and DVC).
6. Whether the `pg` / `none pg` flag becomes a `content_rating` column.
7. An ethics-approval reference and the collection method for each source, for the thesis methodology chapter.

## 8. Code

### 8.1 Scripts reproduced in full

These are exact copies of the files or of the commands I ran, taken from disk or from the saved transcript. They are the **recreated** versions I wrote after resets, so they may differ in detail from the originals the earlier numbers came from.

#### 8.1.1 `filter_sheng_only.py` (the Sheng flag; copied from the session summary, not from disk)

```python
{read('filter_sheng_only.py').rstrip()}
```

#### 8.1.2 `add_word.py` (add or update a lexicon term, re-flag, rewrite outputs; final version used for `buya`)

```python
{read('scripts/add_word.py').rstrip()}
```

#### 8.1.3 `add_word` first version (used for `chwepe`, 2026-10-08; it also read `sheng_lexicon_clean.csv`, which no longer exists)

```python
{snip['2'].rstrip()}
```

#### 8.1.4 `label_tool.py` (tap-labeling backend)

```python
{read('scripts/label_tool.py').rstrip()}
```

#### 8.1.5 `datefmt.py` (date parser used for the year and era tables in this record)

```python
{read('datefmt.py').rstrip()}
```

#### 8.1.6 `build_unlabeled.py` (builds `sheng_unlabeled_dataset.csv`; also holds the date converter `fmt()` and the meaning rules)

```python
{read('scripts/build_unlabeled.py').rstrip()}
```

#### 8.1.7 Build of `sheng_labeled_dataset.csv` (run inline on 2026-11-14; it executes the top half of `build_unlabeled.py`)

```python
{snip['45'].rstrip()}
```

#### 8.1.8 Inspection of raw date shapes (2026-11-09)

```python
{snip['39'].rstrip()}
```

#### 8.1.9 Labels-by-year analyses run in chat (2026-11-09). The first two were earlier drafts, the third is the version whose table I reported

```python
# --- first pass (regex for 4-digit years only; left 152 rows undated)
{snip['36'].rstrip()}

# --- second pass (adds dd-Mon-yy and mm/yy)
{snip['37'].rstrip()}

# --- third pass (adds emoji-heuristic labels per year)
{snip['38'].rstrip()}
```

#### 8.1.10 `build_record.py` (this document's generator; every table in sections 2 to 6 is computed by it)

{FENCE}python
{open(__file__).read().rstrip()}
{FENCE}

### 8.2 Scripts that existed but are NOT recoverable

I do not have their source. What survives is described below, and nothing else is claimed. To reproduce those steps you would need to rewrite them against these descriptions.

| Script / file | What it did | What survives |
|---|---|---|
| `build_master.py` | Parsed the 7 raw schemas into one table and assigned row ids from the registry | Description only (section 3.1 steps 1 to 3) |
| `clean_master.py`, `compute_exclusions.py` | Normalised text, built `text_display`, flagged and logged exclusions | The five exclusion reasons and their counts (section 3.2); rule thresholds are not recorded |
| `finalize.py` | Assembled the master and the exports | Description only |
| `clean_pii_ascii.py` | Built `text_clean`, `is_empty_after_pii_clean`, `possible_leading_name` | The regexes below, and the order in section 3.7 |
| `emoji_sentiment.py` | Built `emoji_sentiment` | The output only (928 usable rows); the emoji lists are lost |
| `build_queue.py` | Built `label_queue.csv` | The selection criteria in section 4.6 |
| `build_labeling_xlsx.py`, `export_final_schema.py` | Built the stale workbook and `sentiment_dataset.csv` | The output files; export columns `row_count, date, text, sheng_used_and_meaning, label, label_source, platform/source_group, author` (label priority human, then emoji, then blank) |
| `id_registry.pkl`, `human_labels.csv` | Row-id registry and the merged human labels | Gone. The labels survive in `cleaned_population.csv` |

The regular expressions of `clean_pii_ascii.py`, as preserved in the session summary (an excerpt, not the full script):

```python
URL_RE=re.compile(r'https?://\\S+|www\\.\\S+',re.I); MENTION_RE=re.compile(r'@\\w+'); HASHTAG_RE=re.compile(r'#\\w+')
EMAIL_RE=re.compile(r'\\b[\\w.+-]+@[\\w-]+\\.[\\w.-]+\\b'); PHONE_RE=re.compile(r'(?:\\+?254|0)?7\\d{{8}}\\b|\\b\\d{{7,12}}\\b')
NAME_LEAD_RE=re.compile(r'^([A-Z][a-z]+(?:\\s+[A-Z][a-z]+){{1,3}})\\s')
def clean(t): # URL->email->mention->hashtag->phone, then keep only ord(c)<128, collapse whitespace
```

---
*End of record.*
"""
open(O + 'sheng_data_record.md', 'w', encoding='utf-8').write(doc)
print('written', len(doc), 'chars')
`````

### 8.2 Scripts that existed but are NOT recoverable

I do not have their source. What survives is described below, and nothing else is claimed. To reproduce those steps you would need to rewrite them against these descriptions.

| Script / file | What it did | What survives |
|---|---|---|
| `build_master.py` | Parsed the 7 raw schemas into one table and assigned row ids from the registry | Description only (section 3.1 steps 1 to 3) |
| `clean_master.py`, `compute_exclusions.py` | Normalised text, built `text_display`, flagged and logged exclusions | The five exclusion reasons and their counts (section 3.2); rule thresholds are not recorded |
| `finalize.py` | Assembled the master and the exports | Description only |
| `clean_pii_ascii.py` | Built `text_clean`, `is_empty_after_pii_clean`, `possible_leading_name` | The regexes below, and the order in section 3.7 |
| `emoji_sentiment.py` | Built `emoji_sentiment` | The output only (928 usable rows); the emoji lists are lost |
| `build_queue.py` | Built `label_queue.csv` | The selection criteria in section 4.6 |
| `build_labeling_xlsx.py`, `export_final_schema.py` | Built the stale workbook and `sentiment_dataset.csv` | The output files; export columns `row_count, date, text, sheng_used_and_meaning, label, label_source, platform/source_group, author` (label priority human, then emoji, then blank) |
| `id_registry.pkl`, `human_labels.csv` | Row-id registry and the merged human labels | Gone. The labels survive in `cleaned_population.csv` |

The regular expressions of `clean_pii_ascii.py`, as preserved in the session summary (an excerpt, not the full script):

```python
URL_RE=re.compile(r'https?://\S+|www\.\S+',re.I); MENTION_RE=re.compile(r'@\w+'); HASHTAG_RE=re.compile(r'#\w+')
EMAIL_RE=re.compile(r'\b[\w.+-]+@[\w-]+\.[\w.-]+\b'); PHONE_RE=re.compile(r'(?:\+?254|0)?7\d{8}\b|\b\d{7,12}\b')
NAME_LEAD_RE=re.compile(r'^([A-Z][a-z]+(?:\s+[A-Z][a-z]+){1,3})\s')
def clean(t): # URL->email->mention->hashtag->phone, then keep only ord(c)<128, collapse whitespace
```

---
*End of record.*
