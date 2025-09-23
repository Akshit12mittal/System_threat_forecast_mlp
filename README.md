# 🛡️ System Threat Forecaster

## 🏆 Scorecard

* **Final Model:** Tuned XGBoost
* **Final Accuracy:** 0.62510
* **Pass Threshold:** 0.55

## 📌 Project Overview

This project was part of the **Machine Learning Practice (MLP)** course at **IIT Madras**. It was designed as a **full-fledged Kaggle-style competition** with over **1700 students** participating.

The task:
👉 Predict whether a system is likely to get **infected by malware**, using **telemetry data** collected by antivirus software.

We were provided with three main files:

* **`train.csv`** – containing labeled data (features + target)
* **`test.csv`** – unlabeled data for which predictions had to be made
* **`submission.csv`** – the required format for submitting predictions

---

## 🗂️ Dataset Details

* **Training set:** \~100,000 rows × 76 columns
* **Test set:** \~10,000 rows × 75 columns
* **Target variable:** `target` (1 = infected, 0 = not infected)
* **Features:** System hardware, OS configurations, display settings, security measures, etc.

Challenges included:

* Missing values across multiple features
* Skewed numeric distributions (e.g., RAM, disk sizes)
* High-cardinality categorical variables
* Outliers and redundant columns

---

## 🔍 Exploratory Data Analysis (EDA)

Before jumping into models, I **explored the dataset visually** to understand hidden patterns.

### 1. Target Distribution

* **Balanced classes**: Both infected and non-infected systems were reasonably represented.
* This meant accuracy was a valid metric (no major class imbalance).

### 2. Numeric Features

* Plotted **histograms** → many features like RAM and disk size were **right-skewed**.
* **Boxplots** → revealed strong outliers (e.g., unrealistic hardware specs).

📖 *Learning:* These distributions guided how I handled **missing values** (mean vs median) and **outlier treatment** (IQR clipping).

### 3. Categorical Features

* Used **bar plots** to visualize distributions of categorical columns.
* Found that some categories had only 1–2 unique values → candidates for dropping.

📖 *Learning:* Low-cardinality features were encoded with one-hot, while higher ones needed label encoding.

### 4. Correlation Heatmap

* Plotted correlations between numeric features.
* Identified clusters (e.g., display settings correlated with resolution features).

📖 *Learning:* While no single feature was highly correlated with the target, groups of features together contributed predictive power.

---

## 🛠️ Data Preprocessing

Steps applied systematically:

1. **Missing values:**

   * Numeric → mean or median depending on skew
   * Categorical → mode

2. **Feature reduction:**

   * Dropped constant columns
   * Removed duplicate rows

3. **Encoding:**

   * Binary categorical → 0/1
   * Low-cardinality categorical → one-hot encoded
   * Others → label encoded

4. **Outlier handling:**

   * IQR-based clipping of extreme values

5. **Feature alignment:**

   * Ensured `train` and `test` sets had the same features after preprocessing

---

## 🤖 Model Building

I tested multiple models to compare performance.

| Model               | Accuracy   |
| ------------------- | ---------- |
| Dummy Classifier    | 0.5061     |
| Random Forest       | 0.6171     |
| XGBoost (raw)       | 0.6217     |
| SGD Classifier      | 0.5206     |
| **XGBoost (tuned)** | **0.6251** |

### Dummy Classifier

* A simple baseline (majority class prediction).
* Accuracy = **0.5061** → showed that any proper model must beat this.

### Random Forest 🌲

* Handled categorical + numerical mix well.
* Accuracy = **0.6171**.
* Feature importance revealed that **security settings, OS version, and RAM size** were more influential.

### XGBoost ⚡

* Outperformed Random Forest out-of-the-box: **0.6217**.
* Efficient with missing data and imbalanced distributions.

### SGD Classifier

* Struggled with convergence, poor accuracy (**0.5206**).

---

## 🔧 Hyperparameter Tuning

Optimized **XGBoost** parameters:

* `n_estimators` (trees)
* `max_depth` (tree depth)
* `learning_rate` (step size)
* `subsample` and `colsample_bytree` (to avoid overfitting)

✨ Accuracy improved to **0.6251**, comfortably above the pass threshold of **0.55**.

---

## 📊 Visual Insights

Some key visualizations included:

* **Target distribution plot** → confirmed balance in classes.
* **Histograms of numeric features** → revealed skewness and guided imputation.
* **Boxplots** → highlighted outliers.
* **Heatmap of correlations** → showed feature groups (though weak target correlation).
* **Feature importance plot (Random Forest / XGBoost)** → showed that a few system security and configuration settings mattered most.

📖 *These visuals weren’t just pretty — they guided decisions on preprocessing, encoding, and model selection.*

---

## 💡 Learnings

* Always start with **EDA** to guide preprocessing and model choice.
* **Dummy baselines** help measure progress.
* **Tree-based models** (RF, XGBoost) shine with mixed data types.
* Handling **missing values, outliers, and categorical encoding** is as important as the model itself.
* **Hyperparameter tuning** is often the final boost for leaderboard performance.

---
