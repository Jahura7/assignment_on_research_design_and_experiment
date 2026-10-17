# Stroke Risk Prediction Using Machine Learning

A machine-learning project exploring how demographic, physiological, lifestyle, and medical-history features can be used to classify records into the dataset's assigned stroke-risk categories: **Low**, **Moderate**, and **High**.

> **Disclaimer:** This is an educational project, not a medical device. The predictions represent the labels in the supplied dataset and must not be used for diagnosis, treatment, or individual medical decisions.

## Research Documents

This repository contains the research paper, experiment notebook, and presentation slides for the Stroke-Risk Category Classification with Machine Learning project.

### 📄 Research Paper
[Read the Research Paper](paper.md)

### 💻 Experiment Notebook
[View the Colab Notebook](https://github.com/Jahura7/assignment_on_research_design_experiment_and_writing_reseaech_paper/blob/experiment/experiment.ipynb)

### 📊 Presentation Slides
[View the Presentation Slides Image](presentation_slides_view.md)

[Download the Presentation](presentation.pptx)

## Project Overview

Stroke is a serious health concern, and risk-related datasets provide an opportunity to explore how data analysis and machine learning can identify patterns in health-related information. This project investigates and compares two models on a structured stroke-risk dataset:

- **Linear Regression** — included as a simple baseline for comparison. Because the target is categorical, this is not an ideal classification algorithm.
- **Random Forest Classifier** — the primary classification model, selected for its ability to model nonlinear relationships and feature interactions.

The objective is to evaluate how well these models reproduce the dataset's existing risk labels on held-out data, not to claim that they can clinically predict future strokes.

## Dataset

- **File:** `stroke_risk_prediction_dataset.csv`
- **Size:** 50,000 records and 40 columns
- **Target:** `Stroke_Risk`
- **Target classes:** `Low`, `Moderate`, and `High`

The dataset includes features from several groups:

- **Demographics:** age, gender, country
- **Body measurements:** height, weight, BMI
- **Physiological and laboratory measurements:** blood pressure, heart rate, blood glucose, HbA1c, cholesterol, HDL, LDL, triglycerides
- **Lifestyle:** smoking status, alcohol consumption, physical activity, sleep, stress, diet, exercise, daily walking
- **Medical history and conditions:** family history, diabetes, hypertension, heart disease, previous TIA, atrial fibrillation, chronic kidney disease
- **Other information:** medication adherence, work type, residence type, air-pollution exposure, and doctor-consultation need

The notebook excludes `Patient_ID` because it is an identifier, `AI_Health_Recommendation` because it is recommendation text, and `Stroke_Risk_Score` because it may leak information about how the target label was created.

## Project Workflow

1. **Exploratory Data Analysis (EDA)**
   - Inspect dataset dimensions, data types, descriptive statistics, and missing values.
   - Examine the distribution of `Stroke_Risk`.
   - Explore relationships between selected health indicators and the target.
   - Review correlations among numerical features.

2. **Data Cleaning and Preprocessing**
   - Handle missing numerical values with median imputation and categorical values with most-frequent-value imputation.
   - Encode the target labels and categorical predictors.
   - Use a stratified train/test split to preserve class proportions.
   - Standardize features for the Linear Regression baseline; Random Forest does not require feature scaling.

3. **Feature Engineering**
   - `Pulse_Pressure` = `Blood_Pressure_Systolic - Blood_Pressure_Diastolic`
   - `Metabolic_Risk_Index` = `Blood_Glucose * BMI`

   `Metabolic_Risk_Index` is an exploratory engineered feature, not a clinically validated risk score.

4. **Model Training**
   - Train and evaluate Linear Regression as a baseline.
   - Train a Random Forest Classifier using 200 trees, `class_weight='balanced'`, `min_samples_leaf=2`, and `random_state=42`.

5. **Evaluation and Visualization**
   - Compare accuracy, macro precision, macro recall, and macro F1-score.
   - Inspect confusion matrices to understand class-level errors.
   - Plot Random Forest feature importances.
   - Review MAE, RMSE, and R² for Linear Regression cautiously because encoded class labels are categorical rather than a true continuous outcome.

## Evaluation Metrics

| Metric | What it tells us |
|---|---|
| Accuracy | Overall proportion of correct predictions |
| Macro precision | Average precision across classes, treating each class equally |
| Macro recall | Average recall across classes, treating each class equally |
| Macro F1-score | Balance of precision and recall across classes; useful when classes are imbalanced |
| Confusion matrix | Which classes are correctly classified or confused with one another |
| Feature importance | Features that contribute most to the fitted Random Forest's predictions |

Macro-averaged metrics are particularly useful when class frequencies differ. Model performance should be assessed for each risk category, not by accuracy alone.

## Tools and Libraries

The project was developed in **Google Colab** using Python for data analysis, preprocessing, feature engineering, and machine learning model development.

- **Google Colab:** Cloud-based environment for writing and executing Python code.
- **Python:** Primary programming language.
- **pandas:** Data manipulation and analysis.
- **NumPy:** Numerical computations and feature engineering.
- **Matplotlib:** Data visualization.
- **Seaborn:** Exploratory data analysis and statistical visualizations.
- **scikit-learn:** Data preprocessing, model training, and evaluation.

**Development Environment:** Google Colab (Jupyter Notebook interface).

## Repository Structure
Git repository layout:
### main branch
```text
stroke-risk-prediction/
├── README.md
├── literature_review.md
├── methodology.md
```
### experiment branch
```text
stroke-risk-prediction/
├── experiment_stroke_models.ipynb
└── stroke_risk_prediction_dataset.csv
```
 
## How to Run

This project was developed using Python in Google Colab. You can run the notebook in Google Colab or on your local computer.

### Option 1: Run in Google Colab (Recommended)

1. Open this repository on GitHub and switch to the `experiment` branch.
2. Download `experiment.ipynb` and `dataset.csv`.
3. Open [Google Colab](https://colab.research.google.com/).
4. Upload `experiment.ipynb` using **File → Upload notebook**.
5. In the Colab notebook, upload `dataset.csv` using the Files panel.
6. Make sure the dataset filename and file path match the code in the notebook.
7. Run the notebook cells from top to bottom.

If any required library is missing, install the packages in a notebook cell:

```python
!pip install pandas numpy matplotlib seaborn scikit-learn
```

### Option 2: Run Locally

1. Clone or download this repository.
2. Switch to the `experiment` branch and download `experiment.ipynb` and `dataset.csv`.
3. Install Python if it is not already installed.
4. Create and activate a Python environment, if desired.
5. Install the required libraries:

   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn jupyter
   ```

6. Place `experiment.ipynb` and `dataset.csv` in the same folder.
7. Open a terminal in that folder and start Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

8. Open `experiment.ipynb` in your browser and run the cells from top to bottom.

**Note:** The `main` branch contains the project documentation (`README.md`, `literature_review.md`, and `methodology.md`). The `experiment` branch contains the notebook and dataset.
 

## Results

| Model | Accuracy | Macro Precision | Macro Recall | Macro F1-score |
|---|---:|---:|---:|---:|
| Linear Regression baseline | 41.35% | 60.96% | 22.30% | 28.81% |
| Random Forest Classifier | 85.38% | 86.01% | 72.92% | 77.94% |
 
## Limitations

- The target classes are imbalanced, so overall accuracy alone may hide poor performance on a smaller class.
- The dataset's source, label-generation method, and clinical validity have not been independently established in this project.
- Encoding nominal categories with `LabelEncoder` can introduce an artificial ordering. One-hot encoding or a suitable preprocessing pipeline may be preferable.
- Preprocessing steps such as imputation and encoding should be fitted on the training set only to prevent data leakage.
- Linear Regression is not designed for categorical classification; a classifier such as Logistic Regression could provide a more suitable baseline.
- Feature importance indicates model associations, not causation.
- Results on this dataset do not establish clinical usefulness or the ability to predict real-world future stroke events.

## Ethical Considerations

This project is intended for learning and portfolio demonstration. It should not be used to diagnose stroke risk or guide treatment. Any real-world healthcare application would require reliable data provenance, independent validation on representative populations, bias assessment, privacy safeguards, and review by qualified clinical professionals.

## Future Improvements

- Use a scikit-learn `Pipeline`/`ColumnTransformer` so imputation, encoding, and scaling are fitted on training data only.
- Compare Random Forest with Logistic Regression and other appropriate classification models.
- Tune hyperparameters using cross-validation.
- Evaluate per-class precision, recall, F1-score, and confusion matrices.
- Investigate class imbalance using training-only resampling or class-weight strategies.
- Add reproducible model comparison charts and a clear experiment log.

## Author

Created as a machine-learning learning and portfolio project.

## License

This repository contains original project documentation and machine-learning experiments developed for educational and portfolio purposes.

The dataset used in this project was obtained from Kaggle:

- **Dataset:** [Stroke Risk Prediction Dataset](https://www.kaggle.com/datasets/mobeenfatimah/stroke-risk-prediction-dataset)
- **Dataset creator:** Mobeen Fatimah
- **Source:** Kaggle

The dataset remains subject to the license and terms specified by its original creator on Kaggle. Please review those terms before redistributing or reusing the dataset.

Unless otherwise specified, the original code and documentation in this repository are not intended to change the dataset's original license.

## Data Notice

This project uses the Stroke Risk Prediction Dataset from Kaggle for educational, exploratory data analysis, and machine-learning practice.

The dataset is used to explore data preprocessing, feature engineering, classification models, and model evaluation. The results reflect predictions of the labels provided in the dataset and should not be interpreted as validated predictions of actual stroke events.

**Important limitations:**
- The dataset's provenance, label-generation process, and clinical validity have not been independently verified in this project.
- Model performance on this dataset does not establish clinical accuracy or real-world generalizability.
- This project is not intended for medical diagnosis, treatment decisions, or individual health-risk assessment.

Please consult qualified healthcare professionals for medical advice. Always refer to the original Kaggle dataset page for the dataset's licensing terms and source information.
