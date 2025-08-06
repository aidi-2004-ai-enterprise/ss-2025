# Lab 5: Model Implementation and Evaluation Pipeline
This lab builds on the research and decisions from **Lab 4**. You will implement a `training_pipeline.py` Python script that automates the training, evaluation, and comparison of your selected models for the binary classification task using the [Company Bankruptcy Prediction dataset](https://www.kaggle.com/datasets/fedesoriano/company-bankruptcy-prediction). The pipeline should produce a report to evaluate model performance and guide deployment decisions. Assume the pipeline will be integrated into an AirFlow DAG in a production setting.

## Objectives
Implement the decisions from **Lab 4** to:
- Preprocess the dataset.
- Train and evaluate three models: a benchmark model (e.g., Logistic Regression) and two additional models (e.g., Random Forest, XGBoost, CatBoost).
- Generate a comprehensive report comparing model performance to assess overfitting, underfitting, and deployment readiness.

## Requirements
Your `training_pipeline.py` must include the following components. Use jot notes to summarize your approach for each in your report (max 4 jot notes per component, as in **Lab 4**).

1. **Exploratory Data Analysis (EDA)**:
   - Perform EDA on the dataset’s features to understand distributions, correlations, and potential issues (e.g., missing values, outliers).
   - Visualize key patterns (e.g., histograms, box plots, correlation heatmaps).
   - Justify how EDA informed your preprocessing or feature selection decisions.
   - Why: Ensure data quality and guide preprocessing/feature engineering.

2. **Data Preprocessing**:
   - Apply preprocessing techniques from **Lab 4** (e.g., normalization, encoding, handling missing values, outlier treatment).
   - Based on your research, will you address class imbalance? (e.g., SMOTE, undersampling, class weighting) and justify your approach.
   - Show how PSI graphs between train and test. 
   - Ensure train/test sets are stratified and free of sampling bias 
   - Why: Prepare data consistently for all models to ensure fair comparisons.

3. **Feature Selection (PLEASE KEEP IT SIMPLE)**:
   - Implement a feature selection method from **Lab 4** (e.g., correlation filtering, xgboost feature importance, or regularization).
   - Report the selected features for each model and justify their retention/removal.
   - Address multicollinearity if detected.
   - Why: Reduce noise, improve model performance, and enhance interpretability.

4. **Hyperparameter Tuning (PLEASE KEEP IT SIMPLE)**:
   - Tune hyperparameters for each model using a method from **Lab 4** (e.g., sklearn random search, optuna).
   - Document the tuning process and final hyperparameters for each model.
   - Balance computational efficiency with model performance.
   - Why: Optimize model performance for the dataset.

5. **Model Training**:
   - Train three models: the benchmark model and two additional models chosen in **Lab 4**.
   - Use a cross-validation strategy (e.g., stratified K-fold) to ensure robust training.
   - Save trained models for evaluation.
   - Why: Compare performance across models to select the best candidate.

6. **Model Evaluation and Comparison**:
   - Evaluate all three models on training and test sets to assess overfitting/underfitting.
   - Generate the following for each model and training/test set, overlap the results for training and test set:
     - **Calibration Curve**: Plot to assess probability reliability.
     - **Brier Score**: Quantify calibration performance.
     - **ROC-AUC Curve**: Measure discrimination ability.
   - Compare models in a table summarizing metrics (e.g., ROC-AUC, Brier Score, F1 Score) across train/test sets.
   - Why: Ensure models generalize well and are suitable for deployment.

7. **SHAP Values for Interpretability**:
   - Compute SHAP values for the best-performing model to explain feature contributions.
   - Visualize SHAP summary plots (e.g., bar or beeswarm plots).
   - Discuss how SHAP insights align with business needs (e.g., regulatory compliance).
   - Why: Provide interpretable insights for stakeholders.

8. **Population Stability Index (PSI)**:
   - Calculate PSI between train and test sets to detect data drift for key features.
   - Report PSI values and interpret their implications for model reliability.
   - Suggest actions if significant drift is detected (e.g., retraining, re-sampling).
   - Why: Ensure model stability in deployment.

9. **Challenges and Reflections**:
   - Document the main challenges faced during implementation (e.g., computational constraints, data quality issues, model instability).
   - Reflect on how you addressed these challenges and what you learned.
   - Why: Demonstrate problem-solving and critical thinking.

## Deliverables
1. **training_pipeline.py**:
   - A Python script that automates the entire pipeline: EDA, preprocessing, feature selection, hyperparameter tuning, model training, evaluation, SHAP analysis, and PSI calculation.
   - Output a report (e.g., PDF, markdown, or console output) summarizing:
     - EDA visualizations and insights.
     - Selected features and preprocessing steps.
     - Hyperparameter tuning results.
     - Model comparison table (metrics across train/test sets).
     - Calibration curves, ROC-AUC curves, Brier Scores.
     - SHAP summary plots.
     - PSI results and drift analysis.
   - Ensure the script is reproducible (e.g., set random seeds, document dependencies).
   - FOLLOW PEP STANDARDS! Doc strings, type hinting, use `ruff` from here: https://docs.astral.sh/ruff/
   - You do not need to add unit tests, however note in real life adding unit test for functions that are used to evaluate the model is encouraged.

3. **5-Minute Video**:
   - Walk through your pipeline and report, explaining key decisions and results.
   - Focus on rationale, not code details. Do **not** read word-for-word to avoid mark deductions.
   - Highlight challenges and how you addressed them.

## Notes
- Ensure consistency with **Lab 4** decisions (e.g., model choices, preprocessing methods).
- Use libraries like `pandas`, `scikit-learn`, `SHAP`, `matplotlib`, `seaborn`, `xgboost`, `catboost` as needed.
- If you deviate from **Lab 4** decisions, justify why in your report.
- Test your pipeline to ensure it runs without errors.
- Marks will be deducted for unclear, redundant, or incomplete submissions.

## Questions to Address in Your Report
- What were the main challenges you faced during implementation, and how did you overcome them?
- How did your **Lab 4** research influence your **Lab 5** implementation?
- Based on your evaluation, which model would you recommend for deployment, and why?
