# Comprehensive Real-World Git Workflow Lab Assignment

## Scenario
You are part of a development team.

You will be in a team of 3 people.

1 person will create an empty public repository through github's UI.:


Each team (3 members) will fork this repository, collaborate on features, and submit pull requests (PRs) to their team fork. 

You’ll simulate professional workflows, including branching, code reviews, merging, rebasing, undoing changes, unstaging etc.

## Objectives
- Install Git and set up a GitHub account.
- Fork and manage a team repository using GitHub Flow.
- Implement realistic workflows: branching, PRs, code reviews, merging, rebasing, undoing changes etc.
- Collaborate in teams, including pair programming and code review metrics.
- Create learning artifacts (cheatsheet, workflow diagram, branch visualization).
- Evaluate Git practices and document retrospectives.

## Activity

### Part 1: Setting Up Git and GitHub
1. **Install Git**:
   - **Windows**: Download from [git-scm.com](https://git-scm.com), install with defaults. Verify: `git --version`.
   - **macOS**: Run `brew install git`. Verify: `git --version`.
   - **Linux (Debian/Ubuntu)**: Run `sudo apt update` and `sudo apt install git`. Verify: `git --version`.

2. **Create a GitHub Account**:
   - Sign up at [GitHub](https://github.com) and confirm your email.
   - Add an SSH key (Settings > SSH and GPG keys).

3. **Configure Git**:
   - Set user details:
     ```bash
     git config --global user.name "Your Name"
     git config --global user.email "your-email@example.com"
     git config --global init.defaultBranch main
     ```
   - Verify: `git config --list`.

### Part 2: Team Setup and Repository Fork
1. **Form Teams**:
   - Instructor assigns teams of 3. Designate one member as the **Owner**.

2. **Add teammates to Repository** (Fork Owner):
   - Add team members as collaborators (Settings > Collaborators > Add people).
   - Clone the repository made by the owner.
   - Add upstream remote (optional):

4. **Set Up PR Template** (Fork Owner):
   - Create `.github/pull_request_template.md`:
     ```markdown
     ## What Changes Were Made
     Describe the changes in this PR.

     ## Why These Changes Were Needed
     Explain the purpose or issue addressed.

     ## Testing Performed
     List tests or steps to verify the changes.

     ## Screenshots (if applicable)
     Attach relevant screenshots.
     ```
   - Add, Commit and push:
     ```bash
     mkdir -p .github
     # Add pull_request_template.md
     git add .github/pull_request_template.md
     git commit -m "chore: added PR template"
     git push -u origin main
     ```

5. **Set up a venv**:
    - Use UV and create a pyproject.toml file which will be used to create a virtual environment.
    - Ensure xgboost and pandas verion is pinned to the latest semver.

    Push the pyproject.toml, and uv.lock file to the remote repository.

5. **Create a main.py** (Fork Owner):
    - Create a script that loads data from penguins dataset.

### Part 3: GitHub Flow and Workflows - All working on the same file (Dependant changes)
Adopt **GitHub Flow**: Feature branches are created from `main`, PRs are reviewed and merged quickly, and deployments are frequent. Each member works on a task:

```
    - split the dataset (Person A)
    - Create a default xgboost model (Person B)
    - do a .fit() on the dataset (Person C)
```

#### Workflow 1: Creating a Feature Branch and PR
1. **Create a Feature Branch**:
   - Update `main`:
     ```bash
     git checkout main
     git pull origin main
     ```
   - Create a branch (e.g., `feat-<your-name>-<task>`):
     ```bash
     git checkout -b feat-<your-name>-<task>
     ```

2. **Make Changes**:
    - Edit the `main.py` based on your role:

    ```
        - split the dataset (Person A)
        - Create a default xgboost model (Person B)
        - do a .fit() on the dataset (Person C)
    ```


3. **Push and Create PR**:
   - Push:
     ```bash
     git push -u origin feat-<your-name>-task
     ```
   - Create a PR targeting `main` using the PR template. Assign a teammate as reviewer.

#### Workflow 2: Reviewing a PR
1. **Review** (Reviewer):
   - Check **Files changed**, leave two comments (e.g., style suggestion, logic question).
   - Approve or request changes.

2. **Address Feedback** (Author):
   - Make changes:
     ```bash
     git checkout feat-<your-name>-task
     # Edit files
     git add <files>
     git commit -m "fix: addressed review feedback"
     git push origin feat-<your-name>-task
     ```

#### Workflow 3 Merging a PR:

Each person A, B and C will create a PR which is merged into main. 
You will run into issues here because we cannot merge all 3 into main at once. 
You will need to create a C into B and B into A and then A into main.

This is an example of how working on 1 file with lots of dependancies can lead to complex merging techniques.


#### Workflow 4 - All working on parallel changes (independant changes)

Now that you've merged 1 branch into main.py, each person will work on their own file:

Each person will create their own new main.py and open a PR that is not merged into main. 

You must show the following:

Start by ensuring your local main.py is synced with remote before you create your new branch. 

Add a commit to your main.py and push the changes.

Workflow 5:

The owner will update the main.py 

Workflow 6:

The PRs created in workflow 4, which have not been merged yet should be rebased with the latest main.py.