# Datasheet: Reddit First-Time Homebuyer Supplementary Dataset
**Role in project:** Held-out real-world generalisation test set — Stage 1

---

## Motivation

**For what purpose was the dataset created?**
This dataset was compiled to serve as a real-world generalisation test for the
intent classifier trained on Zillow data. It contains organic buyer posts from
Reddit — unseen during training — to evaluate whether the model transfers
from curated dialogue data to naturalistic buyer language.

**Who created the dataset?**
Compiled by the project author (Surya, Walsh College QM640 capstone) using
the Arctic Shift Reddit API. No credentials were required for data collection.

**Who funded the creation?**
Self-funded academic research. No external grant.

---

## Composition

**What do the instances represent?**
Each instance is a Reddit post from a first-time homebuyer or real estate
enquirer, representing an organic, unsolicited buyer communication.

**How many instances are there?**
4,038 unique posts.

**What are the fields?**
| Field | Description |
|---|---|
| `post_id` | Reddit post identifier |
| `subreddit` | Source subreddit (`r/FirstTimeHomeBuyer` or `r/RealEstate`) |
| `title` | Post title |
| `body_excerpt` | Truncated post body |
| `first_contact_text` | Concatenation of title + body — primary inference field |
| `score` | Reddit upvote score |
| `num_comments` | Number of comments on the post |
| `created_utc` | UTC timestamp of post creation |
| `url` | Direct Reddit URL |
| `intent_category` | Empty — no labels (100% null by design) |
| `urgency_tier` | Empty — reserved for Stage 2 (100% null) |
| `notes` | Empty |

**Is there a label associated with each instance?**
No. This dataset is intentionally unlabelled. It is used for inference and
qualitative generalisation assessment only — not for supervised training
or quantitative evaluation.

**Are there recommended data splits?**
No splits. The full 4,038 rows are used as a single held-out inference set.

**Are there any errors, sources of noise, or redundancies?**
- Posts may contain informal language, slang, abbreviations, and grammatical
  irregularities typical of Reddit writing style
- Some posts may discuss topics tangentially related to home buying rather
  than direct buyer enquiries
- Body excerpts are truncated — very long posts are not fully represented
- Deleted or removed post bodies appear as empty strings and are filtered
  during preprocessing

---

## Collection Process

**How was the data collected?**
Posts were collected using the Arctic Shift Reddit API
(`https://arctic-shift.photon-reddit.com`), which provides access to
historical Reddit data without OAuth credentials.

**What was the sampling strategy?**
Monthly time-chunked requests from January 2020 to March 2023.
Posts were filtered to two subreddits: `r/FirstTimeHomeBuyer` and
`r/RealEstate`. Distribution is approximately even across months.

**Over what timeframe was the data collected?**
Post creation dates: January 2020 — March 2023.
Data was scraped in April 2025 for this project.

**Were any ethical review processes conducted?**
Reddit posts are publicly available content. No personally identifiable
information (PII) beyond Reddit usernames (not retained) was collected.
No IRB process was conducted; the data is treated as public domain text
consistent with academic fair use norms.

**Did individuals consent to data collection?**
Reddit's public API terms permit access to publicly posted content for
research purposes. Individual poster consent was not separately obtained,
consistent with standard practice for public social media research.

---

## Preprocessing / Cleaning / Labeling

**What preprocessing is applied in this project?**
- `first_contact_text` used as the inference field (title + body concatenation)
- Lowercasing
- URL removal
- Special character stripping
- Whitespace normalisation
- Rows with fewer than 3 tokens dropped

**Were labels assigned?**
No labels were assigned. This dataset is permanently held out as an
unlabelled generalisation test set.

---

## Uses

**What tasks is this dataset appropriate for?**
- Qualitative generalisation testing of NLP classifiers trained on formal
  real estate dialogue data
- Distribution shift analysis between curated and organic buyer language
- Supplementary real-world validation in academic NLP capstone projects

**Are there tasks for which this dataset should NOT be used?**
- Supervised training (no labels)
- Quantitative F1 or accuracy evaluation (no ground truth)
- Any application making consequential decisions about individual Reddit users
- Commercial use without review of Reddit's current terms of service

---

## Distribution

**How is the dataset distributed?**
Stored in the GitHub repository for this project:
`https://github.com/Ayrus-5858/inbound-lead-nlp`
under `data/supplementary_buyer_enquiries.csv`

**Is the dataset distributed under a license?**
The compiled dataset is made available for academic use under the project
repository's licence. Underlying content remains subject to Reddit's
Terms of Service (`https://www.redditinc.com/policies/user-agreement`).

---

## Maintenance

**Who maintains the dataset?**
The project author. This is a static snapshot — no updates are planned.

**Will the dataset be updated?**
No. It is a fixed held-out test set. Modifying it after model training would
invalidate the generalisation evaluation.

---

## Project-Specific Notes

- The `first_contact_text` column is the only field used during inference
- This dataset is loaded AFTER model training is complete and locked
- It must never be used to tune hyperparameters or inform any training decision
- Avg confidence score from the classifier on this set will be compared to
  the Zillow test set confidence as a distribution shift indicator
