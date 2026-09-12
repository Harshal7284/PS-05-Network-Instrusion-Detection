# 🛡️ Network Intrusion Detection: Predictive Cybersecurity Classification

![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Cybersecurity](https://img.shields.io/badge/Cybersecurity-Network_Flows-red?style=for-the-badge)

> **Addressing ML Neuronets 2.0 - Problem Statement 05: Cybersecurity**[cite: 2]  
> *Classifying network-flow activity as benign or as one of the relevant attack categories.*[cite: 2]

---

## 📖 Table of Contents
1. [🎯 Problem Statement Overview](#-problem-statement-overview)
2. [📂 Dataset](#-dataset)
3. [🛠️ Data Preprocessing & Feature Engineering](#️-data-preprocessing--feature-engineering)
4. [⚖️ Addressing Class Imbalance](#️-addressing-class-imbalance)
5. [🧠 Model Selection & Hyperparameter Tuning](#-model-selection--hyperparameter-tuning)
6. [📈 Evaluation & Results](#-evaluation--results)

---

## 🎯 Problem Statement Overview

Modern networks generate massive volumes of connection and traffic information, making the manual identification of malicious activity incredibly difficult[cite: 2]. The goal of this project is to build a robust machine-learning detector capable of distinguishing normal network behavior from different forms of suspicious or attack-related activity[cite: 2].

A successful solution goes beyond simply fitting a default classifier[cite: 2]. It requires a reliable preprocessing pipeline, careful handling of mixed feature types, noisy variables, and uneven class frequencies[cite: 2]. Furthermore, the model must be validated to ensure it recognizes attacks without overfitting or generating excessive false alarms[cite: 2].

---

## 📂 Dataset

The analysis and predictive modeling are performed using the **`DrDoS_DNS.csv`** dataset, which contains detailed records of network traffic flows[cite: 1].

---

## 🛠️ Data Preprocessing & Feature Engineering

To build a consistent preprocessing-to-model pipeline, the data underwent rigorous cleaning and preparation steps[cite: 1, 2]:

* **🧹 Noise Handling:** Empty and constant columns were stripped from the dataset, and infinite values (`inf` / `-inf`) were replaced to stabilize the training environment[cite: 1].
* **🏷️ Target Encoding:** The target variable was mapped into a binary format: `BENIGN` was set to `0`, and `DrDoS_DNS` (Attack) was set to `1`[cite: 1].
* **✂️ Redundancy Reduction:** A correlation matrix was utilized to identify highly correlated features[cite: 1]. 5 features (including `total_forward_packets_length` and `backward_packet_length_mean`) with a correlation threshold above `0.95` were dropped to reduce feature noise[cite: 1].
* **🔠 Categorical Handling:** One-Hot Encoding was applied to process categorical variables effectively[cite: 1].
* **📏 Scaling:** Following a stratified 80-20 train-test split, the feature matrix was standardized using `StandardScaler`[cite: 1].

---

## ⚖️ Addressing Class Imbalance

The original training dataset exhibited severe class imbalance:
* 🔴 **Attack Instances:** 96.6%[cite: 1]
* 🟢 **Benign Instances:** 3.3%[cite: 1]

To prevent the model from becoming biased toward the majority attack class, **SMOTE (Synthetic Minority Over-sampling Technique)** was applied to the training data[cite: 1]. This synthetically oversampled the minority class, resulting in a perfectly balanced **50/50** training distribution[cite: 1].

---

## 🧠 Model Selection & Hyperparameter Tuning

The core modeling algorithm selected for this classification task was the **`RandomForestClassifier`**[cite: 1]. To optimize performance and prevent overfitting, hyperparameters were systematically tuned using **`RandomizedSearchCV`**[cite: 1].

| Setup / Parameter | Value Used |
| :--- | :--- |
| **Validation Strategy** | 3-Fold Cross-Validation[cite: 1] |
| **Scoring Metric** | `f1_macro` *(Evaluates fairly across both classes, fulfilling domain expectations)*[cite: 1, 2] |
| **`n_estimators`** | 50[cite: 1] |
| **`max_depth`** | 5[cite: 1] |
| **`min_samples_split`** | 10[cite: 1] |
| **`min_samples_leaf`** | 5[cite: 1] |
| **`criterion`** | `entropy`[cite: 1] |

---

## 📈 Evaluation & Results

The tuned Random Forest model was evaluated on unseen test data, yielding exceptional detection capabilities:

### 🏆 Classification Performance
* The model achieved a **precision, recall, and F1-score of 1.00** for both the `BENIGN` and `ATTACK` classes[cite: 1].
* By maintaining perfect recall and precision across both classes, the model successfully navigated the complex trade-off between accurately detecting attacks and minimizing false alarms[cite: 1, 2].

### 🌟 Top 5 Predictive Features
An analysis of feature importances revealed the top drivers of the model's 100% accuracy[cite: 1]:

1. `total_backward_packets` **(37.3%)**[cite: 1]
2. `total_backward_packets_length` **(24.5%)**[cite: 1]
3. `backward_iat_mean` **(17.6%)**[cite: 1]
4. `flow_bytes_per_seconds` **(9.5%)**[cite: 1]
5. `forward_packet_length_mean` **(5.5%)**[cite: 1]
