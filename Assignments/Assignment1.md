# Data Stack Problem-First Comparison Assignment

## Assignment Overview

**Objective**: Analyze data tools/platforms by focusing on the problems they solve and their real-world context. Choose between comparing competing tools or analyzing how complementary tools form an ecosystem. This assignment emphasizes a *problem-first, tool-second* approach to help you understand why tools exist and how to evaluate them critically, avoiding buzzword-driven learning.

**Format**: 6–8 slide presentation + 1-page analysis document + hands-on demo evidence

**Weight**: 10% of final grade

**Due**: 1 week from assignment date.

**Submission**:
- Slide deck as PDF via LMS
- (mp4 file) Screen recording presenting your slidedeck with face webcam and audio enabled. (Have all three enabled, facecam, audio and screenrecord. And present your powerpoint slidedeck.)
- Demo evidence (screenshots or code snippets) in presentation appendix

## Core Philosophy

Many students focus on the “latest and greatest” tools, chasing buzzwords like “scalable” or “AI-powered” without understanding the problems these tools solve. This assignment forces you to think *problem-first*, critically evaluate marketing claims, and explore either competitive tools or collaborative ecosystems. By defining problems and requirements, you’ll learn to make informed technology decisions.

## Task Options

**Choose ONE of the following:**

### Task A: Competitive Tool Analysis
Compare **two competing tools/platforms** that address the same problem within a chosen domain.

### Task B: Ecosystem Integration Analysis
Analyze how **two complementary tools** work together to solve a multi-stage problem within a chosen domain.

---

## Task A: Competitive Tool Analysis

Choose **one domain** (Data Engineering, DevOps, or MLOps) and compare **two tools/platforms** that solve the same problem with different approaches. Your goal is to:
1. Define the fundamental problem and its requirements.
2. Analyze how each tool addresses the problem, including one marketing claim vs. reality.
3. Identify trade-offs and best-fit use cases.
4. Create a simple demo to explore one tool’s functionality.

### Domain Options for Task A
- **Data Engineering**:
  - Data warehousing: Snowflake vs. Google BigQuery
  - Data transformation: dbt vs. SQL Mesh
  - Workflow orchestration: Airflow vs. Prefect
- **DevOps**:
  - Container orchestration: Kubernetes vs. AWS ECS
  - CI/CD: GitHub Actions vs. Jenkins
  - Infrastructure as code: Terraform vs. CloudFormation
- **MLOps**:
  - Experiment tracking: MLflow vs. Weights & Biases
  - Model training: SageMaker vs. Vertex AI

*Note*: Tools must directly compete (solve the same problem). Consult the instructor for custom tools.

---

## Task B: Ecosystem Integration Analysis

Choose **one domain** and analyze how **two complementary tools** work together to solve a multi-stage problem. Your goal is to:
1. Define the multi-stage problem and its requirements.
2. Analyze each tool’s role and their integration, including one marketing claim vs. reality.
3. Identify integration challenges and best-fit use cases.
4. Create a demo showing one tool’s role in the ecosystem.

### Domain Options for Task B
- **Data Engineering**:
  - **Problem**: Data pipeline from ingestion to transformation
  - **Ecosystems**: Fivetran + dbt; Snowflake + Airflow
- **DevOps**:
  - **Problem**: Automating application deployment
  - **Ecosystems**: Docker + GitHub Actions; Terraform + AWS ECS
- **MLOps**:
  - **Problem**: ML model training and deployment
  - **Ecosystems**: MLflow + Kubernetes; DVC + BentoML

*Note*: Tools must be complementary (e.g., one for ingestion, one for transformation). Consult the instructor for custom ecosystems.

---

## Presentation Structure

### Slide 1: Title Slide
- Title, name, task (A or B), domain, tools, course details, date.

### Slide 2: Problem Definition
- **Task A**: Core problem the tools solve.
- **Task B**: Multi-stage problem the ecosystem addresses.
- Describe stakeholders, challenges, and consequences. Include a brief scenario.

### Slide 3: Requirements
- List 4–5 requirements (e.g., scalability, cost-efficiency).
- **Task B**: Include one integration requirement (e.g., data compatibility).

### Slides 4–5: Tool Overview and Analysis
- **Task A** (1 slide per tool):
  - Description, target users, key differentiator.
  - How it solves the problem; one marketing claim vs. reality.
  - Trade-offs and use cases.
- **Task B** (1 slide for both tools):
  - Role in ecosystem, integration points.
  - One marketing claim vs. reality for one tool.
  - Integration challenges and use cases.

### Slide 6: Decision Framework
- **Task A**: Framework for choosing between tools (2–3 key questions, e.g., “What’s the budget?”).
- **Task B**: Guidelines for implementing the ecosystem (2–3 considerations, e.g., “How to handle failures?”).
- Visual summary (e.g., table, flowchart).

### Slide 7: Reflection
- How did the problem-first approach shape your analysis?
- How will you evaluate tools in the future?

### Slide 8: References
- 3–4 credible sources in APA format.

### Appendix: Demo
- **Task A**: A “hello world” demo (e.g., dbt model, Jenkins pipeline).
- **Task B**: One tool’s role (e.g., Airflow DAG; optional: both tools).

---

## Requirements

- **Length**: 6–8 slides (excluding appendix).
- **Sources**: 3–4 credible sources (e.g., documentation, technical blogs).
- **Citations**: APA format.
- **Demo**: Basic demo with evidence in appendix (screenshots or code; optional 1-minute video).
- **Format**: Clear, visual slides (use diagrams/tables; avoid text-heavy slides).

---

## Grading Rubric

| **Criteria**                | **Excellent (90–100)**                                                                 | **Good (80–89)**                                                             | **Satisfactory (70–79)**                                                   | **Needs Improvement (<70)**                                              |
|-----------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------|
| **Problem & Requirements (30%)** | Clear, contextual problem and requirements; strong scenario.                          | Good problem and requirements; clear scenario.                               | Basic problem/requirements; weak scenario.                                 | Vague problem/requirements.                                              |
| **Analysis (30%)**             | **Task A**: Clear comparison, trade-offs, critiques claim.<br>**Task B**: Clear integration, challenges, critiques claim. | **Task A**: Solid comparison, some trade-offs.<br>**Task B**: Good integration, some challenges. | **Task A**: Basic comparison.<br>**Task B**: Basic integration.            | **Task A**: Shallow comparison.<br>**Task B**: Poor integration.          |
| **Framework (20%)**            | Practical framework with clear visual.                                                | Useful framework, good visual.                                               | Basic framework, unclear visual.                                           | Vague framework.                                                         |
| **Reflection & Demo (10%)**    | Thoughtful reflection; clear demo.                                                    | Good reflection; functional demo.                                            | Basic reflection; minimal demo.                                            | Weak reflection; poor/missing demo.                                       |
| **Presentation & Research (10%)**| Engaging slides, quality sources, excellent citations.                               | Organized slides, good sources.                                              | Adequate slides, limited sources.                                          | Poor slides, weak sources.                                               |

---

## Success Tips

- **Start with the Problem**: Define the problem before researching tools.
- **Task A**: Highlight trade-offs (e.g., cost vs. flexibility).
- **Task B**: Sketch the workflow first; focus on how tools connect.
- **Critique Hype**: Use user reviews to check marketing claims (e.g., “Is Snowflake really ‘zero maintenance’?”).
- **Keep It Simple**: Use diagrams for workflows; demos can be basic (e.g., Docker container).
- **Check Resources**: Use tutorials from tool websites or blogs for demo setup.

## Resources
- **Tutorials**: dbt (docs.getdbt.com), Docker (docs.docker.com), MLflow (mlflow.org/docs).
- **Blogs**: Netflix, Uber engineering blogs.
- **Forums**: Stack Overflow, Reddit (r/dataengineering, r/devops).
- **Citations**: Purdue OWL for APA format.

## Learning Outcomes

1. Develop a problem-first mindset for evaluating tools.
2. Analyze tools/ecosystems based on requirements and trade-offs.
3. Gain basic hands-on experience with data/DevOps/MLOps tools.
4. Present technical analysis clearly.
