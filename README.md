# Amazon Gift Card Reviews — Sentiment & Emotion Analysis

MBAX6418 · AI assignment work on the **Amazon 2023 Reviews** public dataset (Gift Cards category, 152,410 reviews).

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

## Files

| File | What it is |
|---|---|
| `Gift_Cards.jsonl.gz` | Source data (public, mcauleylab.ucsd.edu) |
| `NRC-Emotion-Lexicon-Wordlevel-v0.92.txt` | NRC emotion word list (official download) |
| `sample*.json` / `labels*.json` / `nrc_answers.json` | Sampling, labels, and NRC scoring intermediates |
| `labeled_reviews_100.csv` / `labeled_reviews_balanced_150.csv` | Final labeled datasets |
| `sentiment_dashboard.html` / `sentiment_dashboard_v3.html` | Dashboards (self-contained) |

All sampling uses fixed seed **42** (disjoint batches).
