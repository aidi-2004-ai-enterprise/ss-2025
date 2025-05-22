# Comprehensive Real-World Git Workflow Lab Assignment

## Scenario
You are part of a development team.

You will be in a team of 3 people.

1 person will create an empty public repository through github's UI.:


Each team (3 members) will clone this repository, collaborate on features, and submit pull requests (PRs). 

You’ll simulate professional workflows, including branching, code reviews, merging, rebasing, undoing changes, unstaging etc.

## Objectives
- Install Git and set up a GitHub account.
- Clone and manage a team repository using GitHub Flow.
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

### Part 2: Team Setup and Repository Clone
1. **Form Teams**:
   - Instructor assigns teams of 3. Designate one member as the **Owner**.

2. **Add teammates to Repository** (Repo Owner):
   - Add team members as collaborators (Settings > Collaborators > Add people).
   - Clone the repository made by the owner.
   - Add upstream remote (optional):

4. **Set Up PR Template** (Repo Owner):
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

5. **Set up a venv** (Repo Owner):
    - Use UV and create a pyproject.toml file which will be used to create a virtual environment.
    - Ensure xgboost and pandas verion is pinned to the latest semver.

    ```
    uv init .
    uv sync
    ```

    Push the pyproject.toml, and uv.lock file to the remote repository.

6. **Create a main.py** (Repo Owner):
    - Create a script that loads data from penguins dataset.

### Part 3: GitHub Flow and Workflows - All working on the same file (Dependant changes)
Adopt **GitHub Flow**: Feature branches are created from `main`, PRs are reviewed and merged, and deployments are frequent. Each member works on a task:

```
    - split the dataset (Person A)
    - Create a default xgboost model (Person B)
    - do a .fit() on the dataset (Person C)
```

#### Workflow 1: Creating a Feature Branch and initial PR
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

#### Workflow 2: Reviewing and updating PR
1. **Review** (Reviewer):
   - Check **Files changed**, leave two comments (e.g., style suggestion, logic question).
   - Approve or request changes.

2. **Address Feedback** (Author):
   - Accept 1 suggested change from reviewer:
     ```bash
     git checkout feat-<your-name>-task
     # Edit files
     git add <files>
     git commit -m "fix: addressed ___"
     git push origin feat-<your-name>-task
     ```

#### Workflow 3 Merging 1 PR at a time:

**Must be done in the following order**:

Person A puts up a PR.
Others review it. 
Once approvals are acquired, it is merged in main using the UI on github.

**NOTE**: Do not pull the most recent changes from main or master branch. I recognize this isn't best practices. This lab's intention is to see how you can handle a situation where your local main is out of sync with the remote main, and therefore your branch. This futher means the PR that each person (B,C) has is branched off of a main that is stale. This is common, and requires your PR to be merged with a local main which is back in sync. Follow the steps below to manager this scenario.

Person B's PR is now out of date. (This is expected behaviour)

Person B must merge their local branch by:
- Check out to main branch
- Pull latest changes from main origin
- Checkout back to their feature branch
- Merge their feature branch with main local. 
- Push changes to feature branch origin.
- Create a PR to merge their feature branch to main. Following the steps from Person A above.

Person C must follow the same steps as Person B from above.


I reccomend everyone sit down together and do it in person after class. If you need help you can reach out. 