# Lab 3: Penguins Classification with XGBoost and FastAPI

## Overview
This individual assignment focuses on building a machine learning pipeline using the Seaborn penguins dataset, training an XGBoost model, and deploying it via a FastAPI application. You will preprocess the data, train and evaluate a model, and create a prediction endpoint with proper input validation. The assignment emphasizes robust error handling, logging, and professional coding practices.

## Learning Objectives
- Load and preprocess a dataset using one-hot encoding and label encoding.
- Train and evaluate an XGBoost model with parameters to prevent overfitting.
- Build a FastAPI application with a Pydantic data contract for predictions.
- Implement input validation and logging for robust application design.
- Use `uv` for Python package management and maintain a well-organized repository.
- Ensure it's working using the `docs` endpoint.

## Submission Requirements
- **Repository**: Create an individual repository in the `aidi-2004-ai-enterprise` GitHub organization.
- **Package Management**: Use `uv` to manage Python dependencies.
- **Directory Structure**:
  ```
  ├── train.py
  ├── app/
  │   ├── main.py
  │   ├── data/
  │   │   ├── model.json
  ├── pyproject.toml
  ├── README.md
  ```
- **Reference**: Review the `bipin-june11` branch of the [demo-1 repository](https://github.com/aidi-2004-ai-enterprise/demo-1/tree/bipin-june11) for context on issues to address.

## Tasks

### 1. `train.py` Implementation
Create a `train.py` file in the root directory that performs the following:
- **Load Data**: Load the penguins dataset from Seaborn.
- **Preprocess Data**:
  - Apply one-hot encoding to categorical features (`sex`, `island`).
  - Apply label encoding to the target variable (`species`).
  - **Note**: Normalization/standardization is not required for tree-based models like XGBoost.
- **Split Data**: Split the dataset into training and test sets (e.g., 80/20 split, stratified to handle class imbalance).
- **Train Model**: Instantiate an XGBoost classifier with parameters to prevent overfitting (e.g., `max_depth=3`, `n_estimators=100`).
- **Evaluate Model**: Compute F1-score etc. on both training and test sets.
- **Save Model**: Save the trained model to `app/data/model.json`.
  - **Note**: Storing models locally is poor practice; future assignments will use cloud storage.

### 2. `app/main.py` Implementation
Create an `app` folder containing a `main.py` file that implements a FastAPI application with the following:
- **Pydantic Data Contract**: Define a `PenguinFeatures` model for the `/predict` endpoint:
  ```python
  from enum import Enum
  from pydantic import BaseModel

  class Island(str, Enum):
      Torgersen = "Torgersen"
      Biscoe = "Biscoe"
      Dream = "Dream"

  class Sex(str, Enum):
      Male = "male"
      Female = "female"

  class PenguinFeatures(BaseModel):
      bill_length_mm: float
      bill_depth_mm: float
      flipper_length_mm: float
      body_mass_g: float
      year: int
      sex: Sex
      island: Island
  ```
- **Model Loading**: Load the trained model from `app/data/model.json`.
- **Prediction**: Implement a `/predict` endpoint that uses the model to predict the penguin species, ensuring one-hot encoding matches the training process.

### 3. Address Issues from `bipin-june11` Branch
The `main.py` in the `bipin-june11` branch has the following issues to fix:
- **Incorrect One-Hot Encoding**:
  - Issue: The current implementation uses `pd.get_dummies(X_input, columns=["sex", "island"])` without ensuring consistency with training, allowing invalid values (e.g., `island="dummy_value"`) to be encoded.
  - Solution: Use the provided Pydantic `Enum` classes (`Island`, `Sex`) to restrict inputs to valid values and ensure consistent encoding (e.g., save and reuse the encoder from training).
- **Input Validation**: Ensure the application fails gracefully (e.g., returns HTTP 400) with clear error messages for invalid `sex` or `island` values not present in the training dataset.
- **Logging**: Add logging statements in `main.py` using Python’s `logging` module to track model loading, input validation, and prediction outcomes.

## Rubric
**Total Points: 100**

| **Category**                     | **Description**                                                                 | **Points** | **Criteria for Full Points**                                                                                          |
|----------------------------------|--------------------------------------------------------------------------------|------------|---------------------------------------------------------------------------------------------------------------------|
| **Repository Setup**             | Create a repository in the `aidi-2004-ai-enterprise` organization with `uv`.     | **10**     | - Repository follows naming convention.<br>- `pyproject.toml` or `requirements.txt` configured with `uv`.<br>- `README.md` includes clear setup and run instructions. |
| **train.py Implementation**      | Load, preprocess, train, evaluate, and save an XGBoost model.                   | **40**     | - Loads penguins dataset correctly (5 pts).<br>- Applies one-hot encoding for `sex` and `island`, label encoding for `species` (10 pts).<br>- Splits data into stratified train/test sets (e.g., 80/20) (5 pts).<br>- Uses XGBoost with parameters to avoid overfitting (e.g., `max_depth=3`, `n_estimators=100`) (10 pts).<br>- Reports accuracy and F1-score for train and test sets (5 pts).<br>- Saves model to `app/data/model.json` (5 pts). |
| **app/main.py Implementation**   | Create a FastAPI app with a `/predict` endpoint and Pydantic validation.         | **30**     | - Defines `PenguinFeatures` with correct fields and `Enum` for `sex` and `island` (10 pts).<br>- Validates `sex` and `island` to training dataset values (10 pts).<br>- Loads model from `app/data/model.json` and predicts with consistent one-hot encoding (10 pts). |
| **Error Handling**               | Handle invalid categorical inputs gracefully.                                   | **10**     | - Returns HTTP 400 with clear error messages for invalid `sex` or `island` values.<br>- Validates inputs before prediction. |
| **Logging**                      | Include logging in `main.py` for key operations.                                | **10**     | - Uses `logging` module with `INFO` for model loading/predictions, `ERROR` for invalid inputs.<br>- Logs are clear and output to console. |

### Deductions
- **-5 points**: Missing or unclear `README.md`.
- **-5 points per file**: Missing docstrings, type hinting, or poor code readability.
- **-10 points**: Model or API fails to run due to implementation errors.
- **-5 points**: Incorrect categorical encoding in `main.py` (e.g., inconsistent with training).

### General Notes
- Code must include docstrings and type hinting for all functions and classes.
- Code must run without errors for valid inputs.
- Ensure the application handles invalid inputs gracefully (i.e wrong values for island and sex).
- Use the `bipin-june11` branch as a reference for issues to avoid.
