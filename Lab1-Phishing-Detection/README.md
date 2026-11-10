# M115 Lab 1 — Phishing URL Detection

### Exploratory Data Analysis, Logistic Regression & SHAP Explainability

**Module:** M115 — AI & Intelligent Systems  
**Academic Year:** 2026–2027  
**Institution:** ENSIASD Taroudant — Ibn Zohr University

---

## 1. Project Overview

This project focuses on detecting phishing URLs using Machine Learning techniques and explaining model predictions through SHAP (SHapley Additive exPlanations).

The main objective is to develop a classification system capable of distinguishing between legitimate and phishing websites using URL-related and website-related features.

The project follows a complete Machine Learning workflow, including Exploratory Data Analysis (EDA), data preprocessing, Logistic Regression modeling, model evaluation, feature interpretation, and security-oriented threshold analysis.

The results aim to demonstrate how explainable Machine Learning can support cybersecurity analysis and help SOC analysts understand phishing detection decisions.

## 2. Project Objectives

- Explore and understand the phishing URL dataset.
- Perform Exploratory Data Analysis (EDA).
- Identify relevant features for phishing detection.
- Prepare and preprocess the dataset.
- Implement and compare five Logistic Regression configurations.
- Apply different regularization techniques, including L1 and L2.
- Evaluate models using classification metrics.
- Interpret model predictions using SHAP.
- Analyze the impact of different classification thresholds.
- Discuss the results from a cybersecurity perspective.

## 3. Dataset Description

**Dataset:** PhiUSIIL Phishing URL Dataset

**Source:** [Kaggle — PhiUSIIL Phishing URL Dataset](https://www.kaggle.com/datasets/kaggleprollc/phishing-url-websites-dataset-phiusiil)

The dataset contains 235,795 website records and 55 columns in the version used for this project.

It includes URL characteristics, domain information, security indicators, and website-related features.

### Target Variable

The original dataset contains a column named `label`:

| Original label | Meaning |
|---|---|
| 1 | Legitimate website |
| 0 | Phishing website |

For this project, the target variable is transformed into:

```python
df["IsPhishing"] = 1 - df["label"]
```

Therefore:

| IsPhishing | Meaning |
|---|---|
| 1 | Phishing |
| 0 | Legitimate |

This transformation makes phishing the positive class.

## 4. Exploratory Data Analysis (EDA)

Exploratory Data Analysis is performed to understand the dataset structure, examine class distribution, identify missing values, and investigate relationships between features.
### 4.1 Dataset Distribution

The class distribution illustrates the number of legitimate and phishing websites.
<img width="878" height="431" alt="image" src="https://github.com/user-attachments/assets/f7791f64-217d-450a-9697-1ed5cb4ebb8c" /><img width="990" height="490" alt="image" src="https://github.com/user-attachments/assets/3192b5bd-8df1-4c89-acfc-5afc27e92515" />
<img width="700" height="469" alt="image" src="https://github.com/user-attachments/assets/7e0ed040-575e-4f2e-9184-4e858f68202f" /><img width="687" height="469" alt="image" src="https://github.com/user-attachments/assets/b3087a93-fae1-47c4-9a01-9a3545b7865c" /><img width="700" height="469" alt="image" src="https://github.com/user-attachments/assets/228a1e9b-d7a1-4f3e-b05b-1de7ce84e077" />

### 4.2 Feature Correlation Analysis

The correlation heatmap helps identify relationships between numerical features and detect potentially relevant indicators for phishing classification.
<img width="990" height="890" alt="image" src="https://github.com/user-attachments/assets/00911132-b275-40ab-9c38-596d7130d0cc" />

## 5. Data Preprocessing

The preprocessing stage prepares the dataset for Machine Learning.

It includes:

- Creating the binary target variable `IsPhishing`.
- Removing irrelevant identifiers and raw text columns.
- Removing the original target column and precomputed similarity score.
- Constructing the feature matrix `X` and target vector `y`.
- Splitting the dataset into training and testing sets.
- Applying feature scaling where required.

The dataset is divided into:

75% Training set
25% Testing set

To prevent data leakage, preprocessing transformations are fitted using training data only and then applied to the test data.

## 6. Logistic Regression Models

Five Logistic Regression configurations are implemented to study the impact of regularization and model parameters.

The experiments compare different configurations, including:

| **Model** | **Configuration** |
|---|---|
| M1 | Unregularized Logistic Regression |
| M2 | L2 Regularization — C = 0.001 |
| M3 | Standard L2 Regularization — C = 1.0 |
| M4 | L1 Regularization — C = 0.01 |
| M5 | Class-weighted Logistic Regression |

### Model Evaluation

The models are evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- Confusion Matrix
- False Positive and False Negative counts


| **Model** | **Precision** | **Recall (TPR)** | **F1-Score** | **PR-AUC** | **False Positives (FP)** | **False Negatives (FN)** | **Zeroed Weights** |
|---|---:|---:|---:|---:|---:|---:|---:|
| M1 - Unregularized | 0.9996 | 0.9990 | 0.9993 | 1.0 | 11 | 25 | 0 |
| M2 - Strong L2 | 0.9985 | 0.9958 | 0.9971 | 1.0 | 39 | 105 | 0 |
| M3 - Standard L2 | 0.9996 | 0.9989 | 0.9992 | 1.0 | 10 | 28 | 0 |
| M4 - Sparse L1 | 0.9993 | 0.9971 | 0.9982 | 1.0 | 18 | 72 | 24 |
| M5 - SOC Weighted | 0.9995 | 0.9990 | 0.9992 | 1.0 | 13 | 25 | 0 |

The comparison helps identify the most suitable configuration for phishing detection while considering the cost of false alarms and missed threats.

### Feature Weights and Odds Ratios

Model coefficients are analyzed to understand how individual features influence phishing predictions.

Odds Ratios are calculated using:

$$
OR_j = e^{w_j}
$$

This provides an interpretable way to examine how feature values affect the odds of a website being classified as phishing.

## 7. SHAP Explainability

SHAP is used to explain the predictions of the selected Logistic Regression model.

It provides insight into how individual features contribute to the model's output.

### 7.1 Global Feature Importance

SHAP feature importance identifies the features with the greatest overall influence on model predictions.

<img width="911" height="761" alt="image" src="https://github.com/user-attachments/assets/1e8b4b2f-05c2-4cb8-9b5d-2a853bd1686e" />


### 7.2 SHAP Summary Plot

The summary plot illustrates how different feature values influence phishing predictions across the dataset.

<img width="904" height="652" alt="image" src="https://github.com/user-attachments/assets/68f80098-84c1-46cc-906f-24bd11c344af" />

### 7.3 Individual Prediction Explanation

Individual SHAP explanations help identify the features responsible for a specific classification decision.

This is particularly useful in a SOC environment, where analysts need to understand why an alert was generated.

## 8. Security-Oriented Analysis

The project examines model predictions from a cybersecurity perspective.

### 8.1 Confusion Matrix Analysis

The confusion matrix distinguishes between:

- **True Positives (TP):** Phishing websites correctly detected.
- **True Negatives (TN):** Legitimate websites correctly classified.
- **False Positives (FP):** Legitimate websites incorrectly classified as phishing.
- **False Negatives (FN):** Phishing websites incorrectly classified as legitimate.

False positives and false negatives have different operational consequences for security teams.

### 8.2 Classification Threshold Analysis

Different probability thresholds are evaluated to study the trade-off between phishing detection and false alarms.

The selected thresholds are:

| **Threshold** | **TP** | **TN** | **FP** | **FN** | **Precision** | **Recall** | **F1-Score** |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 0.20 | 25221 | 33682 | 31 | 15 | 0.9988 | 0.9994 | 0.9991 |
| 0.50 | 25208 | 33703 | 10 | 28 | 0.9996 | 0.9989 | 0.9992 |
| 0.85 | 25172 | 33713 | 0 | 64 | 1.0000 | 0.9975 | 0.9987 |


The analysis considers:

- A lower threshold for increased phishing sensitivity.
- A standard threshold for balanced classification.
- A higher threshold for stricter automated blocking.

The appropriate threshold depends on the organization's security policy and operational requirements.

## 9. Technologies and Libraries

| Technology | Purpose |
|---|---|
| Python | Programming language |
| Pandas | Data manipulation |
| NumPy | Numerical operations |
| Matplotlib | Data visualization |
| Seaborn | Statistical visualization |
| Scikit-learn | Machine Learning and model evaluation |
| SHAP | Model explainability |
| Jupyter Notebook | Development environment |
| Kaggle | Dataset access and notebook execution |
| GitHub | Version control and project sharing |

## 10. Repository Structure

```text
M115-AI-Intelligent-Systems/
│
└── Lab1-Phishing-Detection/
    │
    ├── M115_Lab1_Phishing_Detection.ipynb
    │
    └── README.md
```

The dataset is not included in this repository. It can be downloaded from the original Kaggle source.

## 11. How to Run the Project

### Option 1: Kaggle

1. Open the notebook in Kaggle.
2. Add the PhiUSIIL Phishing URL Dataset.
3. Verify the dataset path in the notebook.
4. Execute the notebook cells in order.

### Option 2: Local Environment

Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn shap jupyter
```

Download the dataset from Kaggle and update the CSV file path in the notebook.

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Open `M115_Lab1_Phishing_Detection.ipynb` and execute the cells.

## 12. Conclusion

This project demonstrates how Machine Learning techniques can be applied to phishing URL detection.

By combining Exploratory Data Analysis, Logistic Regression, regularization techniques, and SHAP explainability, the project provides both predictive results and insights into the factors influencing classification decisions.

The threshold analysis further highlights the importance of balancing phishing detection performance with false-positive risks in cybersecurity operations.

Overall, this lab illustrates how explainable Machine Learning can contribute to phishing detection and support security analysis in a SOC environment.

---

**Project:** M115 — AI & Intelligent Systems  
**Topic:** Phishing URL Detection using Machine Learning
