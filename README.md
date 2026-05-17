# Heart Disease Cascade Classification System

## Project Overview

This project presents a two-stage Machine Learning cascade architecture for heart disease prediction using the Heart Disease UCI dataset.

Instead of solving the problem with a single model, the system separates the task into two specialized stages:

### Stage 1 — Binary Classification

Determines whether a patient has heart disease or not.

### Stage 2 — Multiclass Classification

If the patient is predicted as sick, a second model estimates the disease severity level.

The idea behind this architecture is inspired by a medical perspective:

Missing a sick patient can be significantly more dangerous than generating additional false alarms.

Therefore, the first model was optimized to prioritize Recall and minimize False Negatives.

---

## Motivation

Initially, these models existed independently.

The multiclass model could estimate severity levels, but performed poorly at detecting disease directly.

Meanwhile, the binary model achieved high Recall (~93–95%), making it more reliable for identifying potentially sick patients.

This led to separating responsibilities:

Patient

↓

Binary Detection Model

Healthy / Sick

↓

If Sick:

↓

Severity Classification Model

↓

Disease severity prediction

Later I discovered this design corresponds to a Cascade Model architecture.

The design emerged from reasoning about the problem and consequences rather than attempting to replicate an existing implementation.

---

## Dataset

Dataset used:

Heart Disease UCI

Target variable:

num

Meaning:

* 0 → No disease
* 1–4 → Presence and severity of heart disease

Dataset preprocessing included:

* Missing value handling
* Feature selection
* One-hot encoding
* Data cleaning
* Binary and multiclass target preparation

---

## Features Used

Selected features:

* age
* trestbps
* chol
* fbs
* thalch
* exang
* oldpeak
* sex (encoded)
* chest pain type
* ECG information

Feature selection was manually controlled to maintain consistency between training and prediction.

---

## Model Architecture

### Model 1: Binary Detection

Algorithm:

Random Forest Classifier

Configuration:

* class_weight="balanced"
* n_estimators=200
* max_depth=7
* min_samples_leaf=6
* max_features="sqrt"

Additional decision:

Custom threshold:

0.34

Reason:

Lowering the threshold reduces False Negatives and increases Recall.

In a medical context:

False Negative > False Positive

---

### Binary Model Performance

Confusion Matrix:

Real Healthy:

56 correctly predicted

28 false positives

Real Sick:

93 correctly detected

7 missed patients

Metrics:

Accuracy: 0.81

Precision: 0.77

Recall: 0.93

F1 Score: 0.84

Interpretation:

The model successfully identifies most patients with disease.

Although precision decreases, this trade-off is acceptable because minimizing missed diagnoses was prioritized.

Top Feature Importances:

1. asymptomatic → 0.221

2. oldpeak → 0.120

3. exang → 0.114

---

### Model 2: Severity Classification

Algorithm:

Random Forest Classifier

Purpose:

Predict severity level among detected sick patients.

Performance observations:

The model performs best on lower severity cases and struggles on higher severity classes.

Possible reasons:

* Class imbalance
* Fewer examples in severe categories
* Increased difficulty separating similar disease states

---

## Project Structure

```text
main.py

data_engineering.py

model_1.py

model_2.py
```

data_engineering.py

Data cleaning and preprocessing

model_1.py

Binary model

model_2.py

Multiclass severity model

main.py

Complete cascade execution pipeline

---

## Technologies Used

Python

Pandas

Scikit-Learn

Random Forest

NumPy

Google Colab

Machine Learning

---

## Future Improvements

Potential improvements:

* Handle uncertainty regions (ambiguous cases)
* Add fallback models
* Evaluate probability-based routing
* Use Pipeline + OneHotEncoder
* Perform hyperparameter optimization
* Improve class imbalance handling
* Test XGBoost and Gradient Boosting methods

---

## Author

Elias Delgado

Machine Learning Student

Current focus:

Machine Learning Engineering and Artificial Intelligence
