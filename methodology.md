%%writefile methodology.md
# Methodology: Machine Learning-Based Predictive Modeling for Stroke Risk Classification

## 1. Refined Research Question
To what extent can a robust machine learning ensemble pipeline optimize the classification of an individual's stroke risk boundary (`Stroke_Risk`) into "Moderate" or "High" categories, utilizing structured physiological features, metabolic interactions, and vascular indicators? More specifically, does explicitly capturing arterial compliance variances (Pulse Pressure) and baseline metabolic indices optimize risk distinction over basic baseline categorical tracking?

## 2. Dataset Description
*   **Target Vector:** `Stroke_Risk` (Moderate, High) translated explicitly into a binary vector (0, 1).
*   **Total Scope:** The raw dataset contains exactly 5,012 patient records mapped horizontally across 40 distinct metadata columns.
*   **Dataset Limitations:** Missing coordinates across critical features (BMI, LDL, Age, Sleep Hours) and high risk of target class imbalances.

## 3. Data Cleaning Plan
*   **Identifier Pruning:** Drop non-predictive fields such as `Patient_ID` and `AI_Health_Recommendation`.
*   **Robust Imputation Strategy:** Numerical missing values will handle median imputation; categorical fields will use mode imputation.
*   **Normalisation:** Apply Z-score normalization (`StandardScaler`) to continuous arrays for regularization balance.

## 4. Feature Engineering Plan
*   **Pulse Pressure Interaction Proxy (\(\Delta\) BP):** Systolic BP minus Diastolic BP to isolate arterial compliance.
*   **Metabolic Risk Index Indicator (MRI):** Blood Glucose multiplied by BMI to capture compound metabolic strain thresholds.

## 5. Models and Selection Rationales
*   **Regularized Logistic Regression:** Serves as a transparent linear baseline estimator providing distinct continuous odds ratios.
*   **Random Forest Classifier:** Handles complex non-linear boundaries, structural multi-collinearity, and gives automated tree-split feature importance ratings.

## 6. Evaluation Metrics and Selection Rationales
*   **Macro F1-Score:** Harmonic mean of precision and recall to evaluate class handling accuracy across unbalanced sample slices.
*   **ROC-AUC Score:** Evaluates structural sorting capability separate from static classification boundaries across thresholds.

