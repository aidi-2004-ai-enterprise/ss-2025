# Assignment 2: Building and Deploying Your ML Application with CI/CD

## 🎯 Learning Objectives
By completing this assignment, you will:
- Implement comprehensive testing strategies for ML applications.
- Build and deploy containerized applications to Google Cloud.
- Write production-level code with high-quality PEP standards, type hinting, docstrings, etc.
- Use Pydantic, FastAPI, Cloud Run, and Secret Key Management.
- Note: You will NOT be making a CI/CD pipeline in this assignment (we'll save that for the final project).
- Perform load testing on production deployments.
- Debug and monitor cloud applications.
- Think critically about scalability, reliability, and cost optimization.

## 📋 Prerequisites
Before starting, ensure you have:
1. **Google Cloud Account**:
    - Create a free trial account [here](https://cloud.google.com/free/docs/free-cloud-features) (requires credit card for verification; no charges during free trial).
    - Create a GCP project (isolated environment for dev, staging, or prod; one project is sufficient).
    - Enable APIs: Cloud Run, Artifact Registry, Google Cloud Storage.
    - Be mindful of usage to avoid exhausting free credits—focus on small-scale testing.
2. **Local Development Environment**:
    - **Docker Desktop** installed from [docker.com](https://www.docker.com/products/docker-desktop/).
    - **Python 3.10.16+** with your Lab 3 environment.
    - **Google Cloud SDK** installed and authenticated (`gcloud init`).
    - **gsutil** installed for Google Cloud Storage interaction.
3. **GitHub Setup**:
    - Repository set up for your project.
    - Familiarize yourself with [GitHub Actions basics](https://docs.github.com/en/actions/quickstart).
4. **Lab 3 Completion**:
    - Trained XGBoost model on the Penguins dataset, saved as a JSON file.
    - Basic FastAPI application with at least one endpoint (e.g., `/predict`).
    - If not complete, revisit Lab 3.

## 🏗️ Architecture Overview
You’ll build a complete ML deployment pipeline:
- FastAPI app serving XGBoost model predictions.
- Model stored in Google Cloud Storage, loaded during inference using the Google Cloud client SDK.
- Dockerized application deployed to Cloud Run via Artifact Registry.
- Load testing to ensure production scalability.

This simulates a real-world ML deployment workflow.

## ⏱️ Time Management
**Estimated Total Time**: 10-15 hours over one week

**Suggested Timeline**:
- **Days 1-2**: Task 1 (Testing) and Task 2 (Containerization)
- **Day 3**: Task 3 (Service account setup and Artifact Registry)
- **Day 4**: Task 5 (Manual Cloud Deployment)
- **Day 5**: Task 7 (Load testing)
- **Day 6-7**: Documentation, review, and final submission

Track milestones to avoid a last-minute rush.

---

## 📝 Step-by-Step Tasks

### Task 1: Unit Testing Foundation (15 points | 1-2 hours)
**Goal**: Ensure your FastAPI app and XGBoost model are reliable and handle edge cases. Upload your trained XGBoost model to Google Cloud Storage as a JSON file.

**Requirements**:
1. Create a `tests/` directory in your project root.
2. Implement ALL 4 unit tests:
    - **Model prediction test**: Test XGBoost model with known penguin data.
    - **API endpoint test**: Test `/predict` returns 200 OK and valid JSON.
    - **Input validation test**: Handle missing or invalid inputs gracefully.
    - **Edge case test**: Test boundary conditions (e.g., extreme values, empty requests).
    - **Additional required tests**:
        - Test for missing fields (e.g., `bill_length_mm` omitted).
        - Test for invalid data types (e.g., strings instead of floats).
        - Test for out-of-range values (e.g., negative `body_mass_g`).
3. Save the trained XGBoost model as JSON and upload it to Google Cloud Storage using the Google Cloud client SDK or manually through the Console UI.
    - In your FastAPI app, load the model during inference using the SDK.
4. **Test Coverage Requirement**: Report your % test coverage using `pytest-cov`. Run tests with coverage and include a coverage report. Example:
    ```bash
    pytest --cov=your_app tests/
    ```
    Ensure your tests comprehensively cover your codebase.

**Code Template**
```python
# tests/test_api.py
from fastapi.testclient import TestClient
from your_app import app  # Import your FastAPI app
import pytest

client = TestClient(app)

def test_predict_endpoint_valid_input():
    """Test prediction with valid penguin data"""
    sample_data = {
        "bill_length_mm": 39.1,
        "bill_depth_mm": 18.7,
        "flipper_length_mm": 181,
        "body_mass_g": 3750
    }
    response = client.post("/predict", json=sample_data)
    assert response.status_code == 200
    assert "prediction" in response.json()

def test_predict_endpoint_invalid_input():
    """Test handling of invalid input"""
    # TODO: Implement this test
    pass

# TODO: Add more tests
```

**Success Criteria**:
- Report your test % coverage (see coverage report).
- All tests pass with `pytest tests/`.
- Tests cover happy path and error cases, including missing fields, invalid types, and out-of-range values.
- Model is uploaded to Google Cloud Storage and loaded correctly during inference.

---

### Task 2: Containerization (20 points | 2-3 hours)
**Goal**: Make your application portable and production-ready using Docker.

**Requirements**:
1. Create a production-ready `Dockerfile` optimized for performance and security.
2. Create a `.dockerignore` file to exclude unnecessary files (e.g., `.env`, `.git`, `__pycache__`).
3. Build and test the container locally.
4. Use `docker inspect` to verify the image’s layers and size after building your image. Include a summary of your findings in your documentation.
5. Document the process in `DEPLOYMENT.md`.

**Steps**:
1. **Write `Dockerfile`**:
    - Use a minimal base image (e.g., `python:3.10-slim`).
    - Install dependencies via `requirements.txt` (generated from `uv pip freeze > requirements.txt`).
    - Copy application code and tests.
    - Expose port 8080 (hardcoded for Cloud Run).
    - Run FastAPI with `uvicorn` or equivalent.
2. **Create `.dockerignore`**:
    ```
    __pycache__
    *.pyc
    .env
    .git
    .gitignore
    *.md
    ```
3. **Local Testing Checklist**:
    - Build image: `docker build -t <your-application-name> .`
    - Run container: `docker run -p 8080:8080 <your-application-name>`
    - Monitor resources: Use `docker stats` for CPU/memory usage. *(This is handy)*
    - Inspect image: `docker inspect <your-application-name>` to review image layers and size.
4. **Document in `DEPLOYMENT.md`**:
    - Build/run commands.
    - Issues encountered and solutions.
    - Performance and security observations.
    - Summary of image layers and size from `docker inspect`.

**Success Criteria**:
- Container builds without errors.
- Endpoints match non-containerized app behavior.
- Image layers and size are reviewed and documented.

---

### Task 3: Set Up GCP Service Account and Artifact Registry (10 points | 1 hour)
**Goal**: Configure a service account for secure access and set up Artifact Registry for container storage.

**Requirements**:
1. Create a GCP service account with necessary permissions.
2. Generate and securely store the service account key.
3. Set up an Artifact Registry repository.
4. Test the local app and container using the service account.

**Steps**:
1. **Create Service Account**:
    - In GCP Console, navigate to IAM & Admin > Service Accounts.
    - Create a service account named `penguin-api-sa`.
    - Assign roles:
        - `Storage Object Viewer`
        - `Storage Bucket Viewer`
    - Generate a JSON key and download it (e.g., `sa-key.json`).
    - Store securely; add to `.gitignore` and `.dockerignore`.
2. **Set Up Environment**:
    - Create a `.env` file locally:
        ```
     GOOGLE_APPLICATION_CREDENTIALS=/path/to/sa-key.json
     GCS_BUCKET_NAME=<your-bucket-name>
     GCS_BLOB_NAME=<your-model-file.json>
     ```
    - In `main.py`, use `load_dotenv()` to load environment variables.
3. **Test Locally**:
    - Run app locally: `python main.py`.
    - Ensure the model loads from Google Cloud Storage using the service account.
4. **Test in Container**:
    - Volume-mount the service account key:
        ```bash
     docker run -d -p 8080:8080 \
       --name <your-new-container-name> \
       -v <your-absolute-path-to-sa-key>:/gcp/sa-key.json:ro \
       -e GOOGLE_APPLICATION_CREDENTIALS=/gcp/sa-key.json \
       <your-docker-image-name>:<tag>
     ```

     Note: usually for `tag` the value is `latest`.

    Notes: 
    ```markdown
        The format is -p host_port:container_port.
        Format: -v host_path:container_path:options.

        Container path: /gcp/sa-key.json:
            - This is where the file will appear inside the container’s filesystem.
            - The container’s application can access the key at /gcp/sa-key.json.

        Option: `:ro`
            - Stands for read-only, meaning the file is mounted in the container with read-only permissions.
            - This enhances security by preventing the container from modifying the key file.
    ```
    - Verify endpoints and model loading.

5. **Create Artifact Registry Repository**:
    - In GCP Console, navigate to Artifact Registry.
    - In the following steps, we'll ensure that you're able to push your image to the registry.

**Success Criteria**:
- Service account created with correct permissions.
- Ran a container with the app *locally*, loading the model using the service account.
- Artifact Registry repository created and accessible.

---

### Task 5: Manual Cloud Deployment (15 points | 1-2 hours)
**Goal**: Deploy your container to Cloud Run manually to understand the process.

**Requirements**:
1. Build and push Docker image to Artifact Registry.
2. Deploy to Cloud Run via GCP Console.
3. Test deployment and document the process.

**Steps**:
1. **Build and Tag Image**:
    - Build for Cloud Run: `docker build --platform linux/amd64 -t penguin-api .`
    - Tag: 
        ```bash
     docker tag penguin-api us-central1-docker.pkg.dev/YOUR_PROJECT_ID/penguin-repo/penguin-api:latest
     ```
2. **Push to Artifact Registry**:
    - Authenticate: `gcloud auth configure-docker us-central1-docker.pkg.dev`.
    - Push: 
        ```bash
     docker push us-central1-docker.pkg.dev/YOUR_PROJECT_ID/penguin-repo/penguin-api:latest
     ```
3. **Deploy to Cloud Run**:
    - In GCP Console, navigate to Cloud Run > Create Service.
    - Select container image from Artifact Registry.
    - Configure:
        - Allow unauthenticated invocations.
        - Port: 8080 (hardcoded).
        - CPU/Memory: Set conservative limits (e.g., 1 CPU, 4 GB).
    - Deploy and note the public URL.
4. **Test Deployment**:
    - Access the `docs/` endpoint.
    - Send a request.

5. **Troubleshoot** (if needed):
    - Check [Cloud Run troubleshooting guide](https://cloud.google.com/run/docs/troubleshooting) for deployment, serving, or connectivity errors.
    - Verify logs in GCP Console.
6. **Document in `DEPLOYMENT.md`**:
    - Commands used.
    - Deployment issues and solutions.
    - Final URL and performance notes.

**Success Criteria**:
- Application deploys to Cloud Run.
- Endpoints accessible via public URL.
- Predictions work correctly.
- Handles reasonable load.

---

### Task 7: Production Load Testing & Analysis (10 points | 1 hour)
**Goal**: Validate Cloud Run deployment under production-scale traffic.

**Requirements**:
1. Install Locust: `uv add locust`.
2. Create `locustfile.py` with realistic user simulation.
3. Run local and cloud load tests.
4. Document results in `LOAD_TEST_REPORT.md`.

**Steps**:
1. **Set Up Locust**:
    - Create `locustfile.py` to simulate POST requests to `/predict`.
    - Run: `uv run locust` (opens web interface at `http://localhost:8089`).
2. **Local Testing**:
    - Test FastAPI app locally with:
        - Baseline: 1 user, 60 seconds.
        - Normal: 10 users, 5 minutes.
3. **Cloud Testing**:
    - Test Cloud Run deployment with:
        - Baseline: 1 user, 60 seconds.
        - Normal: 10 users, 5 minutes.
        - Stress: 50 users, 2 minutes.
        - Spike: Ramp from 1 to 100 users over 1 minute.
4. **Analyze Results**:
    - Create `LOAD_TEST_REPORT.md` with:
        - Response times, failure rates, throughput per scenario.
        - Bottlenecks (e.g., model loading time).
        - Recommendations: Scaling strategies, optimizations.

**Success Criteria**:
- Tests complete without crashing.
- Bottlenecks identified.
- Optimizations proposed.

---

## 📝 Deliverables & Submission

### GitHub Repository
Include:
- Source code with enhancements.
- Test suite (`tests/`).
- Docker configuration (`Dockerfile`, `.dockerignore`).
- CI/CD pipelines (`.github/workflows/`).
- Load testing (`locustfile.py`).
- Documentation (see below).

### Required Documentation
1. **README.md**:
    - Project overview, setup instructions, API documentation.
    - Answers to provided questions (e.g., edge cases, load estimates, security risks).
2. **DEPLOYMENT.md**:
    - Containerization and deployment process.
    - Commands used, issues encountered, solutions.
    - Final Cloud Run URL.
3. **LOAD_TEST_REPORT.md**:
    - Load test results, analysis, recommendations.

### Submission Checklist
- [ ] GitHub repository with all code/workflows.
- [ ] CI/CD pipelines green.
- [ ] Cloud deployment with public URL.
- [ ] Complete documentation.
- [ ] Load test results.

---

## 🏆 Grading Rubric (100 points)
- Task 1: 15 points
- Task 2: 20 points
- Task 3: 10 points
- Task 4: 20 points
- Task 5: 15 points
- Task 6: 20 points
- Task 7: 10 points

---

## 🔧 Troubleshooting Guide
**Cloud Run Issues**:
- See troubleshooting guide: https://cloud.google.com/run/docs/troubleshooting
- It is divided into:
    - Deployment errors
    - Serving errors
    - Connectivity and security errors


**GitHub Actions Failing**:
- Verify GitHub secrets (e.g., service account key).
- Check service account permissions.
- Review Action logs.


**Load Testing**:
- Start with small loads.
- Check rate limits.
- Monitor client/server metrics.


### Answer the following questions in a README document within your repository.

- What edge cases might break your model in production that aren't in your training data?
- What happens if your model file becomes corrupted?

- What's a realistic load for a penguin classification service?
- How would you optimize if response times are too slow?
- What metrics matter most for ML inference APIs?

- Why is Docker layer caching important for build speed? (Did you leverage it?)
- What security risks exist with running containers as root?

- How does cloud auto-scaling affect your load test results?
- What would happen with 10x more traffic?
- How would you monitor performance in production?

- How would you implement blue-green deployment?
- What would you do if deployment fails in production?

- What happens if your container uses too much memory?
