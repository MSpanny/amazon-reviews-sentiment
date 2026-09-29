# Amazon Gift Card Reviews — Sentiment & Emotion Analysis

MBAX6418 · AI assignment work on the **Amazon 2023 Reviews** public dataset (Gift Cards category, 152,410 reviews). This dataset was developed by McAuley Lab in San Diego. Only a portion of the overall dataset was utilized, with focus on the Gift Cards category. 

A blind-labeling pipeline and self-contained interactive dashboards:

- **Sentiment** — POSITIVE / NEUTRAL / NEGATIVE (v3) assigned from title + text only; star ratings were locked away until after prediction.
- **Emotion** — one primary NRC-EmoLex category (anger, anticipation, disgust, fear, joy, sadness, surprise, trust) predicted per review, compared against the emotion of the highest-scoring word per the **NRC-Emotion-Lexicon Wordlevel v0.92** (official download included).
- **Dashboards** — single-file HTML, embedded data, works offline, palette switcher (blue default), no external dependencies. `sentiment_dashboard.html` = 200 reviews · 2-class; `sentiment_dashboard_v3.html` = 150 balanced reviews · 3-class.

## Results

| Set | Sentiment accuracy | Emotion vs NRC agreement |
|---|---|---|
| 100 reviews (2-class, seed 42) | 98.0% (195/200 incl. batch 2) | — |
| 200 reviews (2-class) | 97.5% | 23.2% (151 comparable) |
| **150 balanced (3-class, seed 42)** | **65.3%** — POS 100%, NEU 12%, NEG 84% | 17.3% (110 comparable) |

The balanced set was drawn 50 per *predicted* class from a 600-review blind-labeled pool. The NEUTRAL class collapses (12%) because the 3✩ bucket is nearly empty in this dataset — most "bland" reviews are actually rated 4–5✩.

**Analysis**

The initial run on the data drew the first 100 reviews, which were predominantly positive reviews. Positive reviews are fairly easy to detect, as they typically include positive language, as well as a human tendency to skew to the extremes. With most of the analyzed reviews coming from the positive class, the program appeared to be very accurate. It was only once the reviews were balanced between three categories (Positive, Negative, Neutral) did the deficiencies of the model become exposed. When the model was ran, the sentiment accuracy dropped to 65.3%.

The most difficult class for the model to define is Neutral. The model detected only 12% (6/50) Neutral ratings correctly. When vague text is entered, it comes off as the user has no strong opinion and the model chooses Neutral. More often, the user gives a high rating in this instance.  The model chose Neutral on occurrences when it was Positive on 38/50 reviews. When they give a low rating, they are more likely to enter a thorough description, so vague descriptions are more sparsely linked to low star ratings. 

The emotion ratings show an even greater inconsistency. First, the model was only able to compare emotions on 110/150 ratings. This is simply due to some of the ratings having very little content to draw from, and no words that can be linked to emotion. Next, there were words that skewed the results. For example, the word Gift is included on many reviews, as they are for the category Gift Card. Gift is linked to the emotion of Anticipation in the NRC Word-Emotion Lexicon. Often times the model cued to a different emotion based on the full context of the text entered. It goes to show that there is more to determining the emotion of a reviewer than single word choices. 

The entire process was fairly streamlined to put together. Choosing a Sans Serif type font, and setting a static dark background made the formatting seamless. The LLM created an easy-to-use dashboard that displayed the information in a way that only requires a glance to understand if it hit or miss the predictions. 

## Files

| File | What it is |
|---|---|
| `Gift_Cards.jsonl.gz` | Source data (public, mcauleylab.ucsd.edu) |
| `NRC-Emotion-Lexicon-Wordlevel-v0.92.txt` | NRC emotion word list (official download) |
| `sample*.json` / `labels*.json` / `nrc_answers.json` | Sampling, labels, and NRC scoring intermediates |
| `labeled_reviews_100.csv` / `labeled_reviews_balanced_150.csv` | Final labeled datasets |
| `sentiment_dashboard.html` / `sentiment_dashboard_v3.html` | Dashboards (self-contained) |

All sampling uses fixed seed **42** (disjoint batches).
