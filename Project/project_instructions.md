# Cloud Composer ML Pipeline Assignment

## 📹 **Submission Requirements**
- **Video Demo**: Record a 5-10 minute video showing your working pipeline
- **Must include**: Face cam, audio narration, and screen share
- **Show**: DAG running successfully, explain your model choice, and demonstrate the email notification

## 🎯 **Your Task**
Build an automated ML pipeline using Google Cloud Composer that:
1. Extracts data from BigQuery
2. Trains a simple ML model
3. Sends an email notification when complete

**Important**: You choose your own prediction target and features, but keep it simple!

---

## 1. Environment Setup 🛠️

1. **Follow the Documentation**: Before you begin, successfully follow all the steps outlined in the Google Cloud Composer documentation to create a Cloud Composer 2 environment: [https://cloud.google.com/composer/docs/composer-2/run-apache-airflow-dag](https://cloud.google.com/composer/docs/composer-2/run-apache-airflow-dag)

2. **Grant Permissions**: With the service account created for your Composer environment, you must grant it these roles:
   - **`BigQuery Job User`** - Essential for running queries and creating tables in BigQuery
   - **`Storage Object Admin`** - Required for saving models and metrics to Cloud Storage (you'll need to figure out the exact permissions needed!)

3. **Enable APIs**: Ensure that the **BigQuery API** and the **Cloud Composer API** are enabled for your project. You can check this in the "APIs & Services" dashboard.

4. **Install Additional Packages**: Your DAG will require Python libraries not already installed in the Composer environment. To install them:
   - **Option 1 (UI)**: Use the Composer UI's PyPI packages section to add scikit-learn, xgboost, etc.
   - **Option 2 (Command line)**: Use the `--update-pypi-packages-from-file` flag with the `gcloud composer environments update` command, referencing a `requirements.txt` file

---

## 2. Choose Your Data & Model 📊

**Data Source**: Use the `bigquery-public-data.london_bicycles.cycle_hire` table

**Your Creative Choices** (keep it simple!):
- **Target variable**: What do you want to predict? (e.g., trip duration, start station popularity)
- **Features**: Which columns will help your prediction? (e.g., day of week, hour, weather)
- **Model type**: Start with linear regression or basic classification

**Write a SQL query** that:
- Selects your chosen features and target
- Filters the data appropriately 
- Saves results to a new table in your project

---

## 3. Build Your DAG ⚙️

Create a Python file with three tasks:

### Task 1: Data Extraction
```python
from airflow.providers.google.cloud.operators.bigquery import BigQueryInsertJobOperator

# Use BigQueryInsertJobOperator to run your SQL query
```

### Task 2: Model Training & Persistence
```python
from airflow.operators.python import PythonOperator

def train_model():
    # Connect to BigQuery
    # Load your data FROM THE TABLE created in Task 1
    # Train your model (scikit-learn)
    # Evaluate and print performance metrics to logs
    # SAVE your trained model to GCS bucket (pickle/joblib)
    # SAVE evaluation metrics to GCS as CSV or JSON
    # Return metrics summary for the email notification
```

**Why read from the table instead of passing data directly?**
- **Data persistence**: If training fails, your extracted data survives for debugging
- **Scalability**: Works with large datasets (Airflow's XCom has ~1MB limits)
- **Real-world pattern**: Production ML pipelines always persist intermediate results
- **Inspectable**: You can examine your training data separately in BigQuery

**Model & Metrics Persistence**: Save everything to Cloud Storage for:
- **Model versioning**: Track different model iterations
- **Reproducibility**: Keep metrics for comparison across runs
- **Production readiness**: Models need to be stored for serving

### Task 3: Notification
```python
from airflow.operators.python import PythonOperator
import logging

def log_completion():
    # Log completion message with metrics summary to Cloud Logging
    # Students can view this in GCP Console > Logging
    logging.info("🎉 Model training complete! Check GCS for saved model and metrics")
```

**Bonus Challenge**: Implement email notification using `EmailOperator` (requires SMTP configuration)

**Set Dependencies**: `data_task >> training_task >> notification_task`

**Schedule**: Weekly (`schedule='0 0 * * 0'`)

---

## 4. Deploy & Demo 🚀

1. **Upload DAG**: Put your `.py` file in the Composer DAGs bucket (use version numbers like `my_dag_v1.py`)

2. **Manual Trigger**: In Airflow UI, manually run your DAG for the demo

3. **Record Your Demo**: Show the DAG running, explain your model choices, check Cloud Logging for completion message, and show saved files in GCS

---

## 💡 **Success Tips**
- Start simple - predict something obvious first
- Test your SQL query in BigQuery Studio before putting it in the DAG
- Use clear, descriptive task names
- Check Airflow logs if tasks fail
- Keep your model basic (this is about the pipeline, not ML complexity!)

## ✅ **Grading Focus**
- DAG runs successfully end-to-end
- Clear explanation of your modeling choices
- Professional video demo
- Completion message appears in Cloud Logging
- Model and evaluation metrics properly saved to GCS
- Correct service account permissions configured
- **Bonus**: Email notification implementation