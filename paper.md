# Machine Learning-Based Classification of Stroke-Risk Categories: An Exploratory Study

**Author:** Jahura Begum  
**Repository:** [assignment_on_research_design_experiment_and_writing_reseaech_paper](https://github.com/Jahura7/assignment_on_research_design_experiment_and_writing_reseaech_paper)  
**Study type:** Exploratory supervised machine-learning study

## Abstract

This study examines whether demographic, physiological, lifestyle, and medical-history features can be used to classify the stroke-risk categories assigned in a structured dataset. The dataset contains 50,000 records and 40 columns, with `Stroke_Risk` as the target. Exploratory data analysis was used to inspect the data structure, target distribution, missingness, and selected risk-related variables. A planned modeling experiment compares a rounded Linear Regression baseline with a class-weighted Random Forest classifier using a stratified 80:20 train–test split and accuracy, macro-averaged precision, recall, and F1-score. The executed exploratory analysis found a substantial class imbalance: 68.75% of records were labeled Moderate, 24.22% High, and 7.03% Low. Mean age and mean blood glucose were higher in the High-risk group than in the Moderate-risk group. These descriptive patterns motivate careful class-wise evaluation, but they do not establish clinical risk relationships. Since the saved notebook does not yet contain model-evaluation outputs, comparative predictive performance cannot be concluded from the current artifact. The dataset labels should not be interpreted as verified future stroke outcomes.


