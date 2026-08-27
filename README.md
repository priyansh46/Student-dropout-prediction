# Student Dropout Early-Warning System

A machine learning system that predicts whether an  university student will eventually withdraw from a course using only information available during the first 21 days.

## Objective

 The goal is to estimate early dropout risk so that an institution could prioritize students for academic support.

## Dataset

This project uses the Open University Learning Analytics Dataset (OULAD).

The dataset contains student demographic information, assessment results, and interactions with the Virtual Learning Environment. The original dataset includes student-module records and detailed clickstream activity.

Dataset source:

- https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset
- https://www.nature.com/articles/sdata2017171


## Problem formulation

This is a binary classification problem:

- `1`: student eventually withdrew from the course
- `0`: student did not withdraw; final outcome was Pass, Fail, or Distinction

The model observes only data available before day 21. Students who had already withdrawn before day 21 were excluded because their outcome had already occurred before the prediction point.

## Features

### Behavioral

- Total VLE clicks
- Number of active days
- Activity span
- Average clicks per active day

### Academic

- Number of submitted assessments
- Average assessment score
- Minimum assessment score
- Number of low scores
- Average submission delay

### Enrollment and demographic

- Studied credits
- Previous attempts
- Gender
- Age band
- Region
- Highest education
- IMD band
- Disability indicator

## Models

The project compares:

- Logistic Regression
- Tuned Random Forest
- RBF Support Vector Machine

The Random Forest was selected for the final intervention configuration.

## Results

| Model | ROC-AUC | Dropout Precision | Dropout Recall | Dropout F1 |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.642 | 0.270 | 0.590 | 0.370 |
| Random Forest | 0.681 | 0.320 at threshold 0.50 | 0.490 at threshold 0.50 | 0.390 at threshold 0.50 |
| SVM | 0.677 | 0.310 | 0.570 | 0.400 |

## Selected operating point

The final configuration uses Random Forest with a probability threshold of `0.40`.

| Metric | Value |
|---|---:|
| Dropout recall | 0.688 |
| Dropout precision | 0.280 |
| Dropout F1 | 0.398 |
| True positives | 755 |
| False positives | 1,942 |
| False negatives | 342 |
| True negatives | 2,546 |
| Students flagged | 2,697 |

This operating point prioritizes identifying at-risk students while keeping the intervention list smaller than a very low threshold configuration.

## Explainability

Random Forest feature importance identified the following as the most influential variables:

1. Studied credits
2. Total VLE clicks
3. Average clicks per day
4. Minimum assessment score
5. Average assessment score
6. Average submission delay
7. Total active sessions
8. Activity span
