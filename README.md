# Contradiction-Aware Legal Question Answering Pipeline

## Project Objective
Legal Q&A platforms often contain multiple human-written answers per question, which may vary in quality, completeness, and sometimes even contradict each other.  
The objective of this project is to **systematically filter, evaluate, and select high-quality legal answers** using an **explainable, heuristic-driven pipeline**, without relying on opaque end-to-end generation models.

The pipeline focuses on:
- Identifying **majority legal consensus**
- Removing **contradictory or unreliable responses**
- Selecting the **best answers** based on informativeness, actionability, and readability

---

## Pipeline Overview
The pipeline operates in three major stages:

### 1. Contradiction-Aware Filtering (Question Level)
- Multiple answers to the same legal question are compared using a **Natural Language Inference (NLI)** model.
- Answers are grouped into:
  - **Majority Group**: largest set of mutually agreeing responses
  - **Contradictory Group**: responses that strongly contradict the majority
- A light heuristic rule is applied:
  - If contradictory responses exceed a defined threshold (count and ratio), the entire question is discarded.
  - Otherwise, only majority-consistent answers are retained.

This step improves **data reliability** before any quality scoring is applied.

---

### 2. Heuristic-Based Answer Scoring (Answer Level)
Each retained answer is evaluated independently on three orthogonal dimensions:

#### a) Informativeness
Measures the amount of **substantive legal information**, such as:
- Presence of legal entities
- Case-specific reasoning
- Explanatory depth beyond generic advice

This is implemented using rule-based heuristics inspired by legal content analysis.

#### b) Actionability
Measures how clearly the answer provides **procedural or next-step guidance**, such as:
- Legal remedies
- Authorities to approach
- Filing procedures, notices, or enforcement steps

This captures how usable the advice is in practice.

#### c) Readability
Measured using the **Flesch Reading Ease** metric via the `textstat` library.
- Accounts for sentence length and syllable complexity
- Negative scores are valid and expected for complex legal text
- Higher scores indicate easier comprehension for non-lawyers

---

### 3. Final Answer Selection
For each retained question:
- The answer with the **highest informativeness score** is selected
- The answer with the **highest actionability score** is selected
- The answer with the **best readability score** is selected

Scores are kept separate to preserve **interpretability** and avoid arbitrary weighting.

---

## Repository Contents

### Code
- `contradictiondetection_optimised.py`  
  Implements contradiction detection and majority-consensus grouping using NLI.

- `Combined_Entire_Pipeline_in_one_code.ipynb`  
  End-to-end notebook that runs:
  - Contradiction-aware filtering
  - Heuristic scoring
  - Final answer selection

### Data
- `Compressed_excel_files.zip`  
  Contains the processed datasets used for evaluation and experimentation.

---

## How to Run the Pipeline

1. Clone the repository:
   ```bash
   git clone https://github.com/SharwariPejathaya/legal-qa-contradiction-aware-pipeline.git
