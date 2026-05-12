# Assistive AI for Trustworthy Candidate Answer Grading

## Overview

Manual grading of subjective exam papers is a deeply human task, but it is prone to inevitable errors, fatigue, and inconsistency. This project introduces an **assistive AI layer** designed not to replace Teaching Assistants (TAs), but to support them. By automatically and mathematically identifying highly suspicious grades, the system ensures only the necessary cases require a second look before results are published.

## The Problem

Grading hundreds of open-ended, descriptive answers leads to "grading drift". By the 120th paper, a TA's attention and accuracy drop sharply. Currently, there is no automated consistency check, leaving errors undetected unless a student complains, and leaving students with no objective data to request a re-evaluation.

## Our Solution

We built a pipeline that catches grading anomalies by computing a statistically calibrated prediction interval for what an answer *should* score. If the human evaluator's mark falls outside this interval, the answer is flagged for review.

### Key Features

* 
**Assistive, Not Authoritative:** The system flags suspicious grades; the final decision always remains with the human.


* 
**Multi-LLM Consensus:** Utilizes 5 independent LLMs (Claude, ChatGPT, Gemini, DeepSeek, Llama) to evaluate each answer, leveraging diverse perspectives.


* 
**Mathematical Guarantees:** Employs Conformal Prediction to ensure a 90% coverage guarantee.


* 
**Transparent:** Evaluators see exactly why a flag was raised alongside the confidence intervals.



## System Architecture

The pipeline consists of the following core stages:

1. 
**Data Input:** Processing scanned handwritten answer sheets.


2. 
**OCR:** Claude AI converts the handwriting to text with high fidelity.


3. 
**LLM Evaluation:** 5 independent LLMs score the answers to generate a robust signal.


4. 
**Feature Engineering:** Extracts 12 LLM features (mean, standard deviation, etc.) and 770 SBERT embeddings.


5. 
**Model Training:** A Random Forest (or Neural Network) predicts the expected mark based on the features.


6. 
**Conformal Calibration:** Applies Split Conformal Prediction to generate the final prediction interval.



## Results & Performance

Tested on a dataset of 588 question-answer pairs from 170 answer sheets:

* 
**Recommended Configuration:** Split Conformal Prediction (Split CP) with a Random Forest model using LLM marks.


* 
**Coverage:** Achieved a valid **90.5% coverage** with a tight average width of 1.678.


* 
**Efficiency:** Flagged only ~9.5% (11 out of 116) of answers in the test set for manual review, saving massive amounts of time.


* 
**Offline Fallback:** An SBERT-only mode operates completely locally with zero API costs, maintaining 89.0% coverage.



## Future Work

* 
**Scale Data:** Expand the dataset to 2000+ samples to fully leverage Neural Network (CQR-NN) capabilities.


* 
**Inter-Rater Reliability:** Integrate multi-TA annotations for deeper analysis.


* 
**Deployment:** Develop a web application for TAs to upload sheets and receive instant flags.


* 
**Domain Expansion:** Test cross-course deployment in STEM, law, and humanities.



## Team

* 
**Chavda Mihirsinh** (202301479) 


* 
**Yuva Undaviya** (202301426) 


* 
**Mentor:** Prof. Pritam Anand 



*Project developed as part of PC406 — BTech Mini Project 1 at Dhirubhai Ambani University.*
