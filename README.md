# CFPB Complaint Intelligence System

## Introduction

This project builds an end to end Natural Language Processing system for financial complaint classification using the Consumer Financial Protection Bureau complaint dataset.

The goal is to read a customer complaint narrative and predict the main complaint category so the complaint can be routed, triaged, and prioritized more efficiently.

This project was designed to reflect a real production style machine learning workflow, not just a notebook model demo. It includes:

1. large scale data handling
2. exploratory data analysis
3. label normalization
4. class imbalance handling
5. multiple NLP modeling approaches
6. model comparison
7. inference and risk analysis
8. governance documentation
9. future deployment support

---

## Business Problem

Financial institutions receive large volumes of customer complaints across many product types such as credit reporting, debt collection, mortgages, and bank accounts.

Manually reviewing and routing these complaints creates delays, inconsistencies, and operational risk.

This project answers a practical question:

**Can complaint text alone be used to classify the complaint into the correct business category?**

If successful, this kind of system can support:

1. complaint routing
2. intake automation
3. support triage
4. operational monitoring
5. complaint analytics

---

## Dataset

The project uses the official CFPB Consumer Complaint Database.

Because the raw dataset is very large, the repository does not store the full downloaded file directly.

### Raw dataset characteristics

1. roughly 13.9 million rows
2. 18 columns
3. structured and unstructured complaint data
4. real consumer narratives
5. financial product and issue labels
6. company and response information

### Key columns used

1. `consumer_complaint_narrative`
2. `product`
3. `issue`
4. `company`
5. `state`
6. `timely_response`
7. `company_response_to_consumer`

### Dataset note

Only complaints with non null narratives were used for NLP modeling. This introduced an important limitation:

**Narrative based complaints were effectively web submitted complaints, which introduces selection bias.**

This was documented as part of the project risk analysis.

---

## Project Objective

The primary modeling objective is:

**Input:** complaint narrative text  
**Output:** cleaned complaint product category

Example:

A complaint about fraudulent accounts and identity theft should be predicted as:

`Credit reporting`

---

## Target Design

The original `product` field contained overlapping and inconsistent categories. Similar categories were consolidated into a cleaned target for more stable modeling.

### Example category normalization

1. `Credit reporting, credit repair services, or other personal consumer reports`
2. `Credit reporting or other personal consumer reports`
3. `Credit reporting`

These were merged into:

`Credit reporting`

### Additional mappings included

1. `Credit card or prepaid card` → `Credit card`
2. `Checking or savings account` and `Bank account or service` → `Bank account`
3. `Money transfer, virtual currency, or money service` and related categories → `Money transfer`
4. `Vehicle loan or lease` → `Auto loan`
5. payday and consumer loan categories → `Personal loan`

### Rare class handling

Ultra rare categories were grouped into `Other` to reduce instability and improve learnability.

---

## Final Target Classes

The grouped target categories used in later modeling were:

1. Credit reporting
2. Debt collection
3. Credit card
4. Bank account
5. Mortgage
6. Money transfer
7. Student loan
8. Auto loan
9. Personal loan
10. Other

---

## Project Structure

```text
cfpb-complaint-intelligence/
│
├── app/
│   ├── main.py
│   ├── model_loader.py
│   └── schemas.py
│
├── data/
│   ├── raw/
│   │   └── dataset_download.txt
│   └── processed/
│
├── governance/
│   ├── model_documentation.md
│   ├── risk_assessment.md
│   └── validation_report.md
│
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_training.ipynb
│   ├── 02_training2.ipynb
│   ├── 03_inference_and_risk.ipynb
│   └── 03_training_roberta.ipynb
│
├── src/
│   ├── evaluate.py
│   ├── predict.py
│   ├── preprocess.py
│   └── train.py
│
├── tests/
│   └── test_api.py
│
├── Dockerfile
├── README.md
└── requirements.txt