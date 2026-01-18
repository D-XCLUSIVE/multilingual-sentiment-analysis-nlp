# Sentiment Analysis Using NLP

## Overview
This project implements an end-to-end sentiment analysis system for classifying customer product reviews into **Positive**, **Negative**, and **Neutral** sentiment categories. The work emphasises data quality, linguistic consistency, and rigorous evaluation, recognising that sentiment analysis performance is highly sensitive to dataset integrity and preprocessing decisions.

The system was developed using a structured NLP and machine learning pipeline, combining classical models and neural network architectures, and evaluated using class-wise metrics to account for class imbalance and asymmetric error costs.

**Status:** 🟢 Marked & Final

---

## Problem Definition and Requirements
The objective of this project is to build a reliable sentiment classification system capable of handling real-world data challenges. The dataset contains **20,000 customer reviews** across multiple attributes and presents several key issues:

- Reviews are written in **multiple languages** (English, French, Spanish)
- **Duplicate reviews** exist with conflicting star ratings
- Star ratings must be transformed into **categorical sentiment labels**
- Neutral sentiment is underrepresented, creating **class imbalance**

To ensure valid and generalisable results, the dataset was split into training and validation sets during model development, with the test set reserved strictly for final evaluation to avoid data leakage and overfitting.

---

## Dataset
The dataset consists of customer product reviews with associated star ratings and language metadata. After inspection, three attributes were retained for modelling:
- `review_body` – textual content used for sentiment classification
- `star` – numerical rating used to derive sentiment labels
- `language` – used to ensure linguistic consistency via translation

Raw data are not redistributed in this repository due to usage constraints.

---

## Data Cleaning and Label Engineering
A **context-aware deduplication strategy** was applied to reduce label noise:
- Duplicate reviews with identical star ratings were reduced to a single instance
- Duplicates with star ratings differing by one point retained the higher rating
- Duplicates differing by two or more points were removed entirely due to irreconcilable label conflict

Star ratings were mapped to sentiment labels using a rule-based function:
- **1–2 stars:** Negative  
- **3 stars:** Neutral  
- **4–5 stars:** Positive  

This transformation produced a clearly defined target variable suitable for supervised learning.

---

## Language Handling and Translation Strategy
Initial experiments using multilingual data without translation led to significant performance degradation due to vocabulary fragmentation in English-centric NLP pipelines.

Rather than excluding non-English reviews (which would introduce bias and reduce representativeness), a **translation-based strategy** was adopted:
- French and Spanish reviews were translated into English using Google Translate
- Back-translation was applied to validate translation quality
- Random human checks were conducted to ensure semantic consistency
- Batch processing was used to maintain efficiency and avoid memory overload

This approach improved linguistic consistency, preserved diverse customer perspectives, and increased usable training data.

---

## Text Preprocessing
Two preprocessing pipelines were implemented:

### Classical Machine Learning Pipeline
- Lowercasing and noise removal
- Contraction expansion (e.g. *“don’t” → “do not”*)
- Stop-word removal **with negation preserved**
- Stemming (selected over lemmatisation due to better empirical performance)

Negation handling was explicitly preserved to prevent polarity inversion (e.g. *“not good”*).

### Neural Network Pipeline
- Lowercasing
- Contraction expansion
- Removal of non-alphabetic characters

This lighter preprocessing preserves word order and context for sequence-based learning.

---

## Feature Extraction and Models

### Classical Models
Two feature representations were evaluated:
- **Bag-of-Words (BoW)**
- **TF-IDF**

Three classifiers were trained and compared:
- Logistic Regression
- Multinomial Naïve Bayes
- Support Vector Machine (SVM)

Model selection was guided by **class-wise precision, recall, and F1-score**, rather than accuracy alone.

### Neural Network Models
Two architectures were explored:
- **BiLSTM**
- **Convolutional Neural Network (CNN)**

Hyperparameters such as embedding size, kernel size, number of units, batch size, and epochs were tuned with particular attention to Neutral class recall and overfitting behaviour.

---

## Evaluation Strategy
Final evaluation was conducted on a **held-out test set of 1,500 unseen reviews** using:
- Accuracy
- Precision
- Recall
- F1-score (macro and weighted)

Given class imbalance and linguistic ambiguity, **macro-averaged metrics and class-level recall** were prioritised over raw accuracy.

---

## Results and Key Findings
- TF-IDF consistently outperformed Bag-of-Words across classical models
- Naïve Bayes achieved higher accuracy by favouring dominant classes but performed poorly on Neutral sentiment
- Logistic Regression provided more balanced performance across sentiment classes
- SVM with TF-IDF achieved the most robust overall trade-off between accuracy and class balance
- Neural models performed competitively but did not significantly outperform classical approaches
- Neutral sentiment remained the most difficult class across all models, suggesting intrinsic ambiguity rather than model insufficiency

These results demonstrate that **model complexity alone does not resolve sentiment ambiguity**, and that careful evaluation is essential.

---

## Limitations
- Sentiment labels are inferred from star ratings and may not perfectly align with textual sentiment
- Machine translation may distort idiomatic or subtle sentiment cues
- Neutral sentiment is underrepresented, limiting recall across all models

Future work could explore pretrained language models (e.g. BERT) and class-balancing strategies.

---

## Files
- `/notebook/sentiment_analysis_nlp.ipynb` – Complete NLP pipeline and model evaluation
- `/report/CO4193_report_21357753.pdf` – Final marked submission
- `/data/README.md` – Dataset description and governance

---

## Academic Context
This project was completed as part of postgraduate study in Artificial Intelligence and Data Analytics. The repository presents the final marked submission in a portfolio-ready format, highlighting practical NLP engineering decisions and rigorous model evaluation.
