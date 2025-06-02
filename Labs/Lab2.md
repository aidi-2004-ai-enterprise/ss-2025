# GitHub Actions CI Lab

## Goal
Create a CI pipeline that runs unit tests on every pull request before code can be merged to main.

**Important:** Each person creates their own repository - this is not a group assignment.

## Setup

### Step 1: Create Repository
1. Create a new repository under our organization
2. Name it `ci-lab-[your-name]` 
3. Initialize with a README
4. Clone it locally

### Step 2: Set Up Python Environment
In your main branch:

1. **Initialize UV project:**
   ```bash
   uv init
   ```
   This creates `pyproject.toml` and `uv.lock`

2. **Add dependencies:**
   ```bash
   uv add pandas pytest
   ```

3. **Create main.py:**
   ```python
   import pandas as pd

   def load_penguin_data():
       """Load penguin dataset and return DataFrame shape"""
       # Enter your code here
       return df.shape

   if __name__ == "__main__":
       shape = load_penguin_data()
       print(f"Penguin dataset shape: {shape}")
   ```

4. **Create initial CI workflow:**
   Create `.github/workflows/ci.yml`:
   ```yaml
   name: CI Pipeline

   on:
     pull_request:
       branches:
         - main

   jobs:
     build:
       runs-on: ubuntu-latest
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Set up Python
         uses: actions/setup-python@v5
         with:
           python-version: '3.10'
       
       - name: Install dependencies
         run: |
           pip install uv
           uv sync
       
       - name: Run Python script
         run: uv run python main.py
   ```

5. **Commit and push to main:**
   ```bash
   git add .
   git commit -m "Initial setup with UV and basic script"
   git push origin main
   ```

## Lab Assignment

### Step 3: Create Feature Branch and Tests
1. **Create new branch:**
   ```bash
   git checkout -b add-tests
   ```

2. **Create test file `test_main.py`:**
   ```python
   from main import load_penguin_data

   def test_penguin_data_shape():
       """Test that penguin dataset has expected shape"""
       shape = load_penguin_data()
       
       # Penguin dataset should have 344 rows and 7 columns
       assert shape == (344, 7), f"Expected (344, 7), got {shape}"
   ```

3. **Update CI to include testing:**
   Modify `.github/workflows/ci.yml` to add a test job:
   ```yaml
   name: CI Pipeline

   on:
     pull_request:
       branches:
         - main

   jobs:
     build: # 1. First: ensure code compiles/runs
       runs-on: ubuntu-latest
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Set up Python
         uses: actions/setup-python@v5
         with:
           python-version: '3.10'
       
       - name: Install dependencies
         run: |
           pip install uv
           uv sync
       
       - name: Run Python script
         run: uv run python main.py

     test: # 2. Then: run tests on the built code
       runs-on: ubuntu-latest
       needs: build  # Wait for build to complete
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Set up Python
         uses: actions/setup-python@v5
         with:
           python-version: '3.10'
       
       - name: Install dependencies
         run: |
           pip install uv
           uv sync
       
       - name: Run tests
         run: uv run pytest test_main.py -v

        # Missing deploy job
   ```

4. **Test locally (optional but recommended):**
   ```bash
   uv run python main.py
   uv run pytest test_main.py -v
   ```

5. **Commit and push feature branch:**
   ```bash
   git add .
   git commit -m "Add unit tests and update CI pipeline"
   git push origin add-tests
   ```

### Step 4: Create Pull Request
1. Go to GitHub and create a pull request from `add-tests` to `main`
2. Watch the CI pipeline run automatically
3. Verify both `build` and `test` jobs pass
4. Only merge if all checks are green ✅

## Success Criteria
- [ ] Repository created under organization
- [ ] UV properly configured with dependencies
- [ ] CI runs on every pull request
- [ ] Unit test validates penguin dataset shape
- [ ] Pull request shows passing CI checks
- [ ] Code merged only after tests pass

## Learning Outcomes
- Understand CI/CD workflow with pull requests
- Experience with UV for Python dependency management  
- Write and run unit tests with pytest
- Configure GitHub Actions for automated testing
- See how CI prevents broken code from reaching main branch

## Troubleshooting Tips
- If tests fail, check the dataset URL is accessible
- Ensure all dependencies are in `pyproject.toml`
- Check indentation in YAML files (use spaces, not tabs)
- Review GitHub Actions logs for detailed error messages