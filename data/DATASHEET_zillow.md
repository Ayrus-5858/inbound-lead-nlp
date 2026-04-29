# Datasheet: Zillow Real Estate Conversations
**Role in project:** Primary training dataset — Stage 1 intent classification

---

## Motivation

**For what purpose was the dataset created?**
The dataset was created to support research and development of conversational AI
systems in the real estate domain. It provides labelled buyer-agent dialogue turns
mapped to intent categories, enabling supervised training of intent classifiers.

**Who created the dataset and on behalf of which entity?**
The dataset was published by Zillow Group and released publicly via the
HuggingFace Datasets Hub under the identifier `zillow/real_estate_v1`.

**Who funded the creation of the dataset?**
Zillow Group (commercial entity). No external grant funding is documented.

---

## Composition

**What do the instances represent?**
Each instance is a single conversational turn — specifically the first user message
in a buyer-agent dialogue — paired with an intent category label.

**How many instances are there?**
21,347 rows after extracting first user messages from the full conversation dataset.

**What are the fields?**
| Field | Description |
|---|---|
| `source` | Dataset origin identifier (`zillow_real_estate_v1`) |
| `split` | HuggingFace split tag (all `dialog`) |
| `first_user_message` | Raw text of the buyer's first message |
| `intent_category` | One of 108 intent category labels |
| `urgency_tier` | Empty — reserved for Stage 2 (100% null) |
| `notes` | Empty — reserved for future annotation (100% null) |

**How many labels/classes are there?**
108 unique intent categories. Class distribution is approximately balanced,
ranging from ~97 to ~253 instances per category (mean ~198).

**Is there a label associated with each instance?**
Yes. Every row has a non-null `intent_category` label.

**Is any information missing from individual instances?**
The `urgency_tier` and `notes` columns are entirely null. These are placeholders
for downstream pipeline stages and are dropped before Stage 1 training.

**Are there recommended data splits?**
The original HuggingFace dataset includes a split field. For this project a
fresh stratified 80/10/10 train/validation/test split is applied to ensure
proportional class representation across all 108 categories.

**Are there any errors, sources of noise, or redundancies?**
Conversations are synthetically structured around real estate topics. Messages
are fluent but may be more uniform in style than organic buyer enquiries.
Duplicate messages across intents are possible and screened during cleaning.

---

## Collection Process

**How was the data collected?**
Dialogues were curated by Zillow Group for conversational AI benchmarking.
The exact collection methodology (human authorship vs. templated generation)
is not fully documented in the public release.

**Over what timeframe was the data collected?**
Not disclosed in the public HuggingFace release.

**Were any ethical review processes conducted?**
Not documented in the public release.

---

## Preprocessing / Cleaning / Labeling

**Was any preprocessing done prior to this project?**
Yes — first user messages were extracted from multi-turn conversations.
Only the opening buyer message is retained per conversation.

**What preprocessing is applied in this project?**
- Lowercasing
- URL removal
- Special character stripping
- Whitespace normalisation
- Removal of rows with fewer than 3 tokens
- Deduplication of exact message matches

**Who produced the labels?**
Labels were provided by Zillow Group as part of the original dataset release.
No re-labelling was performed in this project.

---

## Uses

**Has the dataset been used for any tasks already?**
Yes — as a benchmark for real estate conversational AI systems by Zillow Group
and subsequent HuggingFace community projects.

**What tasks is this dataset appropriate for?**
- Intent classification in real estate dialogue systems
- Conversational AI benchmarking
- NLP pipeline development for domain-specific text classification

**Are there tasks for which the dataset should NOT be used?**
This dataset should not be used to make consequential decisions about individual
buyers or sellers without human oversight. It reflects structured dialogue scenarios
and may not generalise to all real-world buyer communication styles.

---

## Distribution

**How is the dataset distributed?**
Publicly available via HuggingFace Datasets Hub:
`https://huggingface.co/datasets/zillow/real_estate_v1`

**Is the dataset distributed under a license?**
License terms are governed by Zillow Group's HuggingFace dataset release.
Users should review the HuggingFace dataset card for current terms of use.

---

## Maintenance

**Who maintains the dataset?**
Zillow Group. The HuggingFace hosted version is maintained by the uploader.

**Will the dataset be updated?**
No update schedule is documented. The version used in this project is the
version available as of April 2025.

---

## Project-Specific Notes

- `urgency_tier` and `notes` columns are dropped before any modelling step
- This dataset is used exclusively for training and internal test evaluation
- It is NOT used for real-world generalisation testing — that role is filled
  by the Reddit supplementary dataset
- HuggingFace dataset card:
  `https://huggingface.co/datasets/zillow/real_estate_v1`
