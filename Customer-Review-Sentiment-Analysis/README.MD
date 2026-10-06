# Customer Review Sentiment Analysis (NLP)

## Overview

This project builds a sentiment classification model on real Amazon customer food reviews, then uses word frequency analysis on negative reviews to surface recurring complaint themes. This combines applied NLP with traditional analytics, mirroring how a product or customer insights team would mine unstructured review text at scale.

## Business Context

A product team cannot manually read hundreds of thousands of customer reviews, but the information inside them, what's working and what's driving complaints, is valuable for prioritizing product and operational improvements. This project builds a model that classifies review sentiment from text alone, and surfaces the vocabulary that defines negative reviews specifically, to help a product team see patterns across a large review volume without reading every single one.

## Objectives

1. Derive a sentiment label (Positive/Negative) from each review's star rating.
2. Clean and preprocess review text for modeling.
3. Train a text classification baseline (TF-IDF + Logistic Regression) and compare it against a neural network model.
4. Evaluate both models with precision, recall, and F1-score per class.
5. Identify recurring themes in negative reviews using word frequency analysis.

## Dataset

- **Source:** Amazon Fine Food Reviews, Kaggle ([link](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews))
- **Grain:** One row per review
- **Rows:** 568,454 initially; 568,401 after dropping rows with null values (mostly missing ProfileName/Summary); 525,763 after removing 3-star (neutral) reviews
- **Fields used:** Text (review body), Score (1-5 star rating, used to derive Sentiment)

## Tools Used

- Python 3
- Pandas
- NLTK (stopwords, tokenization)
- Scikit-learn (TfidfVectorizer, LogisticRegression, classification_report)
- A neural network model (comparison model)
- Matplotlib

## Methodology

### 1. Data Loading and Cleaning
Loaded the dataset and dropped rows with null values (affecting ProfileName and Summary), reducing the dataset from 568,454 to 568,401 rows.

### 2. Sentiment Label Derivation
Removed all 3-star reviews (neutral, excluded as ambiguous), then mapped the remaining star ratings to a binary Sentiment label: 1-2 stars to Negative, 4-5 stars to Positive.

### 3. Text Preprocessing
Applied the following cleaning pipeline to the review text: decoding HTML entities and stripping HTML tags (an iteration added after an initial pass revealed leftover `<br />` tags corrupting the word frequency analysis, see Known Gaps and Next Steps), lowercasing, removing digits, removing punctuation (keeping only letters and whitespace), tokenization, and stopword removal (NLTK English stopword list).

### 4. Train/Test Split and TF-IDF Vectorization
Split the cleaned data 80/20 into training and test sets, then vectorized the cleaned text using TF-IDF.

- **Training set:** 420,610 reviews
- **Test set:** 105,153 reviews
- **TF-IDF vocabulary size:** 166,845 terms (reduced from an initial 188,676 after the HTML-cleaning fix removed the spurious "br" token and related artifacts)

### 5. Model Training
Trained two models on the TF-IDF features: a Logistic Regression baseline, and a neural network as a comparison model.

### 6. Evaluation
Evaluated both models on the held-out test set using precision, recall, and F1-score per class, with particular attention to the Negative class given its minority status.

### 7. Negative Review Theme Analysis
Filtered to the 82,007 negative reviews in the dataset and calculated word frequency across their cleaned text to identify the most common vocabulary.

## Results

### Class Distribution (Test Set)

| Sentiment | Count | % |
|---|---|---|
| Positive | 88,752 | 84.4% |
| Negative | 16,401 | 15.6% |

The dataset is meaningfully imbalanced toward positive reviews, consistent with typical product review platforms.

### Model Comparison

| Model | Class | Precision | Recall | F1-score |
|---|---|---|---|---|
| TF-IDF + Logistic Regression | Negative | 0.66 | 0.91 | 0.76 |
| TF-IDF + Logistic Regression | Positive | 0.98 | 0.91 | 0.95 |
| Neural Network | Negative | 0.65 | 0.92 | 0.76 |
| Neural Network | Positive | 0.98 | 0.91 | 0.95 |

**Overall accuracy:** Logistic Regression 0.91, Neural Network 0.91.

These results are from the final run, after the HTML-cleaning fix described below. An earlier run (before the fix) showed the neural network clearly outperforming Logistic Regression (Negative F1 0.82 vs. 0.76); see Key Findings for why that gap disappeared.

### Top 30 Most Frequent Words in Negative Reviews (After HTML-Cleaning Fix)

| Word | Frequency | Word | Frequency | Word | Frequency |
|---|---|---|---|---|---|
| like | 43,814 | dont | 17,530 | box | 11,740 |
| product | 32,202 | even | 16,342 | time | 11,465 |
| taste | 31,416 | get | 16,057 | dog | 11,383 |
| one | 27,618 | tea | 16,006 | bought | 11,176 |
| would | 26,788 | buy | 14,967 | eat | 10,655 |
| food | 22,413 | really | 14,430 | made | 10,475 |
| coffee | 22,189 | much | 13,935 | bad | 10,357 |
| good | 21,386 | amazon | 13,349 | also | 10,243 |
| flavor | 20,120 | tried | 12,009 | better | 10,241 |
| | | im | 11,946 | try | 10,200 |
| | | first | 11,827 | | |

## Key Findings

**1. An initial word frequency pass surfaced a data cleaning bug, which was found and fixed.** The first run of the negative-review word frequency analysis showed "br" as the single most frequent word (44,109 occurrences), ahead of every genuine content word. This was traced to leftover HTML line-break tags (`<br />`) in the raw review text: the original cleaning pipeline stripped the angle brackets and slash as punctuation but left the letters "br" behind as ordinary text. The pipeline was corrected to strip HTML tags and decode HTML entities before any other cleaning step, and the entire pipeline (word frequency, TF-IDF, and both models) was re-run on the corrected text.

**2. After the fix, the neural network's apparent advantage over the Logistic Regression baseline almost entirely disappeared.** Before the fix, the neural network clearly outperformed Logistic Regression on the Negative class (F1 0.82 vs. 0.76, driven by much higher precision: 0.76 vs. 0.66). After removing the "br" artifact and re-running both models, the two converge to nearly identical performance (Negative F1 0.76 for both; precision 0.65 vs. 0.66; recall 0.92 vs. 0.91). This is a useful lesson in its own right: part of the neural network's earlier apparent edge was tied to how it was handling noise in the data, not necessarily a more genuine understanding of sentiment, and re-evaluating after a data-cleaning fix was essential before concluding one model was reliably better than the other.

**3. Despite class imbalance (84% positive), recall on the minority Negative class remains strong for both models (0.91-0.92).** This indicates the chosen approach (TF-IDF features capturing sentiment-bearing vocabulary) is effective at identifying negative reviews even though they are outnumbered roughly 5-to-1 in the data.

**4. The cleaned vocabulary clearly centers on taste and product quality.** With "br" removed, "taste," "flavor," "good," "bad," "coffee," "tea," and "food" dominate the top 30 list, confirming that disappointment with how a product tastes, rather than issues like shipping, packaging damage, or customer service, is the dominant theme in negative reviews.

**5. Several high-frequency words suggest a "first-time buyer disappointment" pattern.** Words like "tried," "first," "time," and "bought" appearing prominently suggest a meaningful share of negative reviews describe a customer's first experience with a product not meeting expectations, rather than a product that previously satisfied them and later declined in quality.

## Recommendations for the Product Team

- Prioritize taste and flavor quality investigations over logistics or packaging concerns, since taste-related vocabulary dominates negative review language far more than shipping- or condition-related terms.
- Investigate the first-purchase experience specifically (e.g. product description accuracy, flavor expectation-setting), given the frequency of language suggesting disappointment occurs on a customer's first trial of a product.
- Given that Logistic Regression and the neural network now perform almost identically, favor the TF-IDF + Logistic Regression baseline for production use: it is simpler to train, faster to run, and more interpretable (coefficients can be inspected directly), with no meaningful accuracy trade-off observed here.

## Known Gaps and Next Steps

- Topic modeling (e.g. LDA) on negative reviews was not completed in this iteration; only word frequency analysis was performed. Topic modeling would likely separate the taste/flavor theme from other, currently less visible themes (e.g. specific product categories like coffee vs. tea vs. pet food) more precisely than frequency counts alone.
- Consider re-running Logistic Regression with `class_weight='balanced'` if not already applied, and comparing threshold tuning to see whether Negative-class recall can be improved further without the precision trade-off seen in the current baseline.
- Since the neural network's advantage disappeared after the data-cleaning fix, it would be worth testing whether a more substantial neural architecture (e.g. pretrained word embeddings, or a transformer-based model) produces a genuine improvement over the TF-IDF baseline, rather than one driven by data artifacts.

## Repository Structure

```
Customer-Review-Sentiment-Analysis/
│
├── notebook.ipynb
│
└── README.MD
```

## Skills Demonstrated

- Deriving a classification label from an ordinal rating and handling ambiguous (neutral) cases
- Text preprocessing (lowercasing, punctuation and digit removal, tokenization, stopword removal)
- Feature extraction from text using TF-IDF
- Comparing an interpretable baseline model against a neural network for a text classification task
- Evaluating an imbalanced text classification problem using per-class precision, recall, and F1-score
- Identifying a data cleaning bug from exploratory output (an HTML artifact dominating a word frequency chart), fixing it, and re-running the full pipeline rather than reporting the flawed result
- Recognizing that an apparent model performance gap can be an artifact of unclean data, and re-validating a model comparison after a data fix before drawing conclusions
- Translating word-level findings into a product-team-facing recommendation

## Author

Raihan Maulana — Data Analyst Portfolio Project