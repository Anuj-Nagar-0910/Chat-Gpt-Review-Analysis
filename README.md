<div align="center">

# 🤖 ChatGPT Review Analysis

### Sentiment analysis and feature extraction on ~197K Google Play reviews, built in Python

![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![NLP](https://img.shields.io/badge/NLP-TextBlob-green)
![Records](https://img.shields.io/badge/Reviews-196%2C727-blue)
![Period](https://img.shields.io/badge/Period-Jul%202023%E2%80%93Aug%202024-orange)
![Course](https://img.shields.io/badge/Crio-DA--201-red)

</div>

---

## 📑 Table of Contents

1. [Project Overview](#-project-overview)
2. [Headline Numbers](#-headline-numbers)
3. [Dataset](#-dataset)
4. [Methodology](#-methodology)
5. [Key Findings](#-key-findings)
6. [Recommendations](#-recommendations)
7. [Limitations](#-limitations)
8. [Repository Contents](#-repository-contents)
9. [How to Run](#-how-to-run)
10. [Author](#-author)

---

## 🎯 Project Overview

**Business question:**

> *What do users feel about ChatGPT, how strongly do they feel it, and which features or aspects do they most often praise or criticise?*

A star rating shows *how much* users like an app, not *why*. The review text holds the "why", but nobody can read 197,000 reviews by hand. This project uses NLP to turn the free text into measurable insight:

| # | Question | Method |
|---|----------|--------|
| Q1 | What is the **general sentiment**? | Sentiment *polarity* → Positive / Neutral / Negative |
| Q2 | How **strong** is the opinion? | Sentiment *subjectivity* (0 factual … 1 opinionated) |
| Q3 | Which **features** are praised or criticised? | Keyword / phrase frequency, word clouds, aspect analysis |

---

## 📊 Headline Numbers

| Metric | Value |
|---|---|
| Reviews analysed (after cleaning) | **193,148** |
| Average rating | **4.51 / 5** (77% are 5★, 6% are 1★) |
| Sentiment split | **76% Positive · 21% Neutral · 4% Negative** |
| Mean polarity | **+0.45** (scale −1 to +1) |
| Mean subjectivity (reviews with opinion words) | **0.65** |
| Rating vs. polarity correlation (Spearman) | **0.25** (positive but weak) |
| 1–2★ reviews with clearly positive wording | **13%** |

---

## 🗂 Dataset

**Source:** Google Play Store reviews of the ChatGPT Android app, provided as `chatgpt_reviews.csv` (21 MB).

| Column | Description |
|---|---|
| `Review Id` | Unique review identifier |
| `Review` | Free-text review (max 500 characters) |
| `Ratings` | Star rating, 1–5 |
| `Review Date` | Timestamp, 25 Jul 2023 – 23 Aug 2024 |

**Data preparation** (every step is logged in the notebook):

| Step | Result |
|---|---|
| Standardise column names to `snake_case` | `review_id, review, rating, review_date` |
| Fill / drop missing review text | 6 empty reviews removed |
| Convert types | rating → integer, date → datetime |
| Drop duplicate `review_id`s | 3,573 rows removed |
| Duplicate *text* | **Kept** – different users who wrote "good" are separate votes |
| Feature engineering | review length, word count, month, non-ASCII flag, cleaned text |

---

## 🔬 Methodology

1. **EDA** – rating distribution, volume and average rating over time, review length by rating.
2. **Sentiment scoring** – [TextBlob](https://textblob.readthedocs.io/) polarity and subjectivity on the original text. Label: polarity > +0.05 Positive, < −0.05 Negative, otherwise Neutral. TextBlob was chosen because it returns both polarity **and** subjectivity, is fast, and is explainable.
3. **Validation** – sentiment compared against star ratings, with a mismatch analysis and a check on non-English reviews.
4. **Text analysis** – 4–5★ reviews (praise) and 1–2★ reviews (criticism); stop-word removal; top words and two-word phrases (negations kept so "not working" stays distinct); word clouds.
5. **Aspect analysis** – share of positive vs. critical reviews that mention themes such as errors, login, pricing, accuracy, and helpfulness.

---

## 💡 Key Findings

1. **Users like ChatGPT.** Three out of four reviews are positive in wording, and the average rating is 4.51. Sentiment improved slightly over the period (positive share about 71% → 77%).
2. **Opinions are strong both ways.** Subjectivity is high and similar for positive and negative reviews.
3. **Praise is about usefulness.** *good, best, helpful, great, amazing*; phrases such as *best ever, helps a lot, user friendly, helpful students*. Helpfulness and learning is mentioned in 16% of positive reviews vs. 6% of critical ones.
4. **Criticism is mostly about reliability and access, not the AI itself.** *not working, try later, please fix, error occurred, network error, phone number*.

   | Aspect | In 1–2★ reviews | In 4–5★ reviews |
   |---|---|---|
   | Errors & reliability | 18% | 2% |
   | Answer accuracy | 14% | 7% |
   | Login / verification | 11% | 0.3% |
   | Pricing, limits & paywall | 8% | 1% |

5. **Stars and text disagree more than expected.** The correlation is only 0.25, and about 1 in 8 low-star reviews use positive words (probable mis-taps, mixed reviews, typos such as "Nott good").

---

## ✅ Recommendations

1. **Fix reliability first** – clearer error messages and resume/retry when an answer stops midway.
2. **Reduce sign-in friction** – alternative login options and clearer phone-verification guidance.
3. **Make limits and pricing transparent** before users hit them.
4. **Keep improving answer accuracy** – wrong answers appear in roughly 1 in 7 critical reviews.
5. **Reuse the users' own words** (*helpful, user friendly, best for students*) in store copy.
6. **Don't rely on stars alone** – triage 1–2★ reviews by text theme.

---

## ⚠️ Limitations

* TextBlob is lexicon-based: it misses sarcasm, slang and context, and the median review is only 4 words. About 20% of reviews score exactly 0.
* It is English-only. Reviews with emoji or other languages are scored Neutral about twice as often (≈35% vs ≈18%), so the Neutral share is inflated.
* Aspect detection uses simple keyword rules and is indicative, not exact. Most short critical reviews match no aspect, so the percentages are a lower bound.
* One platform (Google Play) and about 13 months of data.

**Ideas for further work:** transformer-based multilingual sentiment, language detection, topic modelling (LDA / BERTopic) on 1–2★ reviews, and linking review spikes to release dates.

---

## 📁 Repository Contents

| File | Description |
|---|---|
| `ChatGPT_Review_Analysis.ipynb` | Main notebook: full analysis with documentation and saved outputs |
| `ChatGPT_Review_Analysis_Video_Script.md` | Talking points for the demo video |
| `chatgpt_reviews.csv` | Raw dataset (196,727 reviews) |
| `chatgpt_reviews_scored.csv` | Cleaned dataset with polarity, subjectivity and sentiment label (generated by the notebook) |

---

## ▶️ How to Run

### Option A – Google Colab
1. Open [colab.research.google.com](https://colab.research.google.com) → **File → Upload notebook** → choose `ChatGPT_Review_Analysis.ipynb`.
2. **Runtime → Run all**. When the data-loading cell asks, upload `chatgpt_reviews.csv`.
3. The notebook installs its own libraries (`textblob`, `wordcloud`, `nltk`, `seaborn`, `scikit-learn`). Run time is about 2 minutes.

### Option B – Locally
```bash
pip install pandas numpy matplotlib seaborn textblob nltk wordcloud scikit-learn jupyter
jupyter notebook ChatGPT_Review_Analysis.ipynb
```
Keep `chatgpt_reviews.csv` in the same folder as the notebook.

> Tip: set `SAMPLE_SIZE = 50_000` in the configuration cell for a faster run.

---

## 👤 Author

**Anuj Nagar**
Crio Data Analytics (DA-201) · Session 5 Assignment

---

<div align="center">

*If you found this useful, feel free to ⭐ the repository.*

</div>
