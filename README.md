# Data-Engineering-Team-Project
This repository hosts the source code and documentation for our team project. It serves as our central workspace for version control, code reviews, and issue tracking.

---

# The task
 
Task 1 - Setup
You must create a git private repository and share it with your teammates.

Task 2 - Extract Transform Load (ETL)
Using a Python Jupyter Lab notebook(s). Write an ETL pipeline to produce a dataset suitable for analysing customer churn (cancelling the auto-renew of their subscription). Your code must be organised using functions and must include Extract, Transform and Load functions where appropriate. Your transform function should perform data quality checks and use feature engineering and other data preprocessing techniques as necessary. Your load function must write a Parquet file containing the final dataset.

Task 3 - Machine Learning (ML)
Using the dataset created in Task 2. Load the dataset and build a ML model to classify churn using the PySpark library. Use a selection of models and use hyper-parameter tuning. Produce confusion matrices and other plots to evaluate your model's performance. 

Task 4 - Submit Files
Run all cells and export the notebooks as HTML. The team leader (only) should submit the HTML files.

Task 5 - Practice your Presentation

---

# 👥 Team Roles & Responsibilities 

| Name & Student ID | Assigned Role | Primary Focus Tasks | Key Deliverables | 
| ----- | ----- | ----- | ----- | 
| **Suraj Chavan** (26001944) | **Project Manager & Git** | Task 1 & Task 5 | Git repository architecture, branch protection rules, commit tracking, presentation coordination. | 
| **Parth Parkhiya** (25107711) | **Data Engineer (Team Leader)** | Task 2, Task 3 & Task 4 | ETL pipeline notebook, missing value imputation, Parquet data export, HTML submission. | 
| **Dhruv Patel** (25117102) | **Data Scientist** | Task 3 | PySpark pipeline, string indexing/vectorization, model training (LR, RF, GBT), hyperparameter tuning. | 
| **Hussnain Umer** (25110803) | **Data Analyst** | Task 2 & Task 3 (Evaluation) | Feature correlation analysis, confusion matrices, ROC curves, evaluation plots, report documentation. |

# 📋 Detailed Description of Each Role

### 1. Project Manager & Git — Suraj Chavan (26001944)

* **The Blueprint:** Serves as the system architect and workflow controller. Sets up project infrastructure, defines Git branch protection rules, enforces peer-review requirements, monitors progress against milestones, and ensures the live presentation runs smoothly.

* **Detailed Responsibilities:**

  * **Repository Setup (Task 1):** Create the private GitHub repository, invite team members, and set up directory structures (`data/`, `notebooks/`, `outputs/`).

  * **Version Control Enforcement:** Maintain branch naming conventions (`feature/etl-pipeline`, `feature/pyspark-ml`, `feature/model-evaluation`), require at least 1 peer review for all Pull Requests (PRs), and prevent direct commits to `main`.

  * **Continuous Integration & Quality Audit:** Check commit history to ensure every team member has sustained and continuous contributions.

  * **Presentation Facilitator (Task 5):** Direct the live presentation structure, coordinate screen-sharing transitions, manage overall timing, and lead the live Git commit history demonstration.

### 2. Data Engineer (Team Leader) — Parth Parkhiya (25107711)

* **The Blueprint:** The data architect responsible for data ingestion, quality validation, feature engineering, storage optimization, and final deliverable compilation.

* **Detailed Responsibilities:**

  * **ETL Pipeline Development (Task 2):** Write clean, modular Python functions inside `notebooks/etl_pipeline.ipynb`:

    * `extract()`: Query SQLite database (`PerfectPetClub.db`) and load auxiliary CSV files (`CRM_report`, `server_log`, `survey`).

    * `transform()`: Clean raw data, handle null/missing values, resolve date inconsistencies, execute schema validation, and engineer aggregated customer metrics.

    * `load()`: Export processed data to compressed columnar Parquet format (`data/processed/processed_churn.parquet`).

  * **Data Quality Control:** Handle missing dates, unreviewed product ratings, optional text fields, and convert raw business rules into the binary target variable `churn`.

  * **Submission Lead (Task 4):** Verify all code cells execute cleanly top-to-bottom, convert `.ipynb` notebooks into HTML format, and perform final task submission on behalf of the team.

### 3. Data Scientist — Dhruv Patel (25117102)

* **The Blueprint:** The machine learning engineer responsible for building the distributed predictive pipeline in PySpark, selecting algorithms, tuning hyperparameters, and optimizing churn classification accuracy.

* **Detailed Responsibilities:**

  * **PySpark Environment Setup (Task 3):** Initialize local `SparkSession` and load `data/processed/processed_churn.parquet` into a PySpark DataFrame.

  * **Feature Preprocessing Pipeline:**

    * Use `StringIndexer` and `OneHotEncoder` to encode categorical columns (e.g., county, subscription tier, payment gateway).

    * Use `VectorAssembler` to assemble all numeric and encoded categorical features into a unified `features` vector.

    * Apply scaling using `StandardScaler` where necessary.

  * **Model Building & Experimentation:**

    * Split data into Training (80%) and Test (20%) sets.

    * Build and compare multiple classifiers: **Logistic Regression**, **Random Forest Classifier**, and **Gradient-Boosted Trees (GBTClassifier)**.

  * **Hyperparameter Tuning:** Implement `ParamGridBuilder` and `CrossValidator` (e.g., tuning `maxDepth`, `numTrees`, `regParam`) to prevent overfitting and optimize prediction accuracy.

### 4. Data Analyst — Hussnain Umer (25110803)

* **The Blueprint:** The domain analyst and visual storyteller responsible for defining feature requirements, auditing model predictions, generating evaluation figures, and translating ML outputs into business insights.

* **Detailed Responsibilities:**

  * **Feature Engineering Collaboration (Task 2):** Identify key churn indicators (e.g., login frequency drops, negative survey sentiment, late delivery history, order recency) to guide the Data Engineer's transformation logic.

  * **Model Performance Evaluation (Task 3):**

    * Calculate model evaluation metrics: **Accuracy, Precision, Recall, F1-Score, ROC-AUC, and PR-AUC**.

    * Analyze class imbalance issues and evaluate business trade-offs (e.g., cost of false positives vs. false negatives in subscriber retention).

  * **Visual Output Generation:** Produce high-resolution charts in `outputs/figures/`:

    * **Confusion Matrices** (annotated heatmap showing True Positives, False Positives, True Negatives, False Negatives).

    * **ROC Curves & Precision-Recall Curves**.

    * **Feature Importance Plots** (ranking top drivers of customer churn).

  * **Peer Review & Presentation Insights:** Review ETL and ML code before PR merges and articulate model trade-offs during the live presentation.

---

# 📊 Raw Input Data & Data Quality Specifications

To perform Task 2, the team will ingest and process raw data from SQLite (`PerfectPetClub.db`), CSV logs, CRM exports, and survey data stored in `data/raw/`.

## 1. Ingested Data Sources & Tables

* **`PerfectPetClub.db` (SQLite Relational Database):**
<img width="1989" height="888" alt="sales_chart" src="https://github.com/user-attachments/assets/43054753-69a0-416d-b285-76ebc1cf3022" />


  * **`customer`**: `id`, `name`, `email`, `password_hash`, `address_line_1`, `address_line_2`, `county`, `postcode`, `subscription_id`

  * **`subscription`**: `id`, `start_date`, `end_date`, `auto_renew`, `subscription_type_id`

  * **`subscription_type`**: `id`, `name`, `delivery_service`

  * **`sale`**: `id`, `order_date`, `expected_date`, `delivered_date`, `customer_id`, `payment_id`

  * **`sale_product`**: `id`, `quantity`, `sale_id`, `product_id`

  * **`product`**: `id`, `name`, `price`, `product_category_id`

  * **`product_category`**: `id`, `name`

  * **`payment`**: `id`, `amount`, `payment_gateway`, `transaction_reference`

  * **`product_review`**: `id`, `star_rating`, `product_id`, `customer_id`, `sale_id`

* **Additional CSV Files:**

  * **`PerfectPetClub_CRM_report.csv`**: Support tickets, customer complaint logs, interaction types.
  <img width="399" height="234" alt="CRM_report_Image" src="https://github.com/user-attachments/assets/95841b31-1fd6-46e8-97d1-7de6a6de78ad" />


  * **`PerfectPetClub_server_log.csv`**: Web/app portal login timestamps, session duration, activity counts.
  <img width="1457" height="367" alt="server_log_Image" src="https://github.com/user-attachments/assets/bbd72dd4-1d29-4288-884e-450f3e38d022" />


  * **`PerfectPetClub_survey.csv` & `PerfectPetClub_survey_questions.txt`**: Customer satisfaction ratings (CSAT), Net Promoter Scores (NPS).
  <img width="888" height="328" alt="survey_Image" src="https://github.com/user-attachments/assets/c99d6c38-0af9-4eb9-8c73-2668d9ccff3b" />


## 2. Missing Data & Quality Checks (Data Engineer & Data Analyst Focus)

* **Delivery Dates (`expected_date`, `delivered_date`):** Impute missing delivery dates for in-transit orders; create boolean feature `is_delayed` where `delivered_date > expected_date`.
 <img width="1222" height="632" alt="sale_missing_dilevery_date_Image" src="https://github.com/user-attachments/assets/3b0164b6-c8c7-4aa8-a225-6efce1baf53c" />

* **Missing Review Ratings:** Impute missing ratings using the mean product rating or user mean rating.

* **Data Type Conversions & Standardizations:**

  * Parse string ISO dates into `datetime64`.

  * Derive binary target variable `churn`: `1` if `auto_renew == 0` or subscription expired without renewal; `0` otherwise.

* **Feature Engineering Outputs:**

  * `total_lifetime_spend`: Sum of all completed payments per customer.

  * `order_frequency`: Total count of orders / customer tenure in months.

  * `avg_rating`: Mean rating left across all purchased items.

  * `delivery_delay_rate`: Percentage of customer orders delivered past the expected date.

  * `days_since_last_order`: Recency of last purchase.

# 🛠 Required Tools & Environment Setup

## Required Libraries & Dependencies:

* **Python Version:** `3.14+`

* **IDE / Workspace:** JupyterLab / Jupyter Notebook

* **Data Engineering:** `pandas`, `numpy`, `sqlite3`, `pyarrow`, `fastparquet`

* **Distributed ML:** `pyspark`

* **Evaluation & Plotting:** `scikit-learn`, `matplotlib`, `seaborn`

---

# 🚀 Getting Started

Follow these step-by-step instructions to set up your local development environment.

## 1. Clone the Repository
Open your terminal (Mac/Linux) or Git Bash (Windows) and clone this private repository to your local machine:
```bash
git clone https://github.com/SC26ACM/Data-Engineering-Team-Project.git
cd Data-Engineering-Team-Project
```

## 2. Installation Script:

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate

# Install required packages
pip install --upgrade pip
pip install pandas numpy pyarrow fastparquet pyspark scikit-learn matplotlib seaborn jupyterlab
```

---

## 🚀 Step-by-Step Git Workflow for All Team Members

To ensure effective team collaboration and clean version control, **never commit directly to the `main` branch**.

### 1. Pull Latest Changes

Always start from an up-to-date `main` branch:

```bash
git checkout main
git pull origin main
```

### 2. Create a Topic/Feature Branch

Create a branch named after your task or role:

```bash
# Suraj (Git Lead)
git checkout -b feature/repo-setup

# Parth (Data Engineer)
git checkout -b feature/etl-pipeline

# Dhruv (Data Scientist)
git checkout -b feature/pyspark-ml

# Hussnain (Data Analyst)
git checkout -b feature/model-evaluation
```

### 3. Commit Work Incrementally

Stage files and write clear, descriptive commit messages matching conventional commit guidelines:

```bash
git status
git add notebooks/etl_pipeline.ipynb data/processed/
git commit -m "feat(etl): add missing value for sale expected date and delivered date"
```

### 4. Push Branch to GitHub

```bash
git push origin feature/etl-pipeline
```

### 5. Open Pull Request & Peer Review

1. Go to `https://github.com/SC26ACM/Data-Engineering-Team-Project`.
2. Click **Compare & pull request**.
3. Set Title: `[ETL] Ingestion, Data Cleaning and Parquet Export`.
4. Assign at least one peer reviewer (e.g., Dhruv or Hussnain).
5. Once approved by the reviewer, merge into `main` and delete the feature branch locally:

   ```bash
   git checkout main
   git pull origin main
   git branch -d feature/etl-pipeline
   ```
---

## 📂 Project Structure

```text
├── data/
│   ├── raw/                    # Raw subscription CSV & db files (Never modify manually)
│   └── processed/              # Output directory for final Parquet files
├── notebooks/
│   ├── etl_pipeline.ipynb    # Task 2: Python / Pandas ETL Pipeline
│   └── pyspark_ml.ipynb      # Task 3: Distributed PySpark Machine Learning
├── outputs/                    # Saved confusion matrices and evaluation charts
├── .gitignore                  # Prevents caching files/virtual environments from being tracked
└── README.md                   # Setup and execution documentation
```

---

# 📌 Task Completion Checklist

### Customer Churn Prediction & Analytics Pipeline

Track team progress across all project deliverables below.

### Task 1: Environment & Repository Setup
- ✅ Create private GitHub repository (`SC26ACM/Data-Engineering-Team-Project`).
- ❌ Add all 4 team members with full contributor access.
- ✅ Define repository structure (`data/`, `notebooks/`, `outputs/`).
- ✅ Establish branch rules (`main` protection, feature branch conventions).
- ✅ Complete comprehensive `README.md` with role matrices, schema, and installation commands.

### Task 2: Extract, Transform, Load (ETL Pipeline)
- ❌ Initialize modular Jupyter notebook (`notebooks/etl_pipeline.ipynb`).
- ❌ **Extract (`extract()`):** Connect to `PerfectPetClub.db` SQLite database and ingest raw CSVs (`CRM_report`, `server_log`, `survey`).
- ❌ **Transform (`transform()`):**
  - ❌ Execute data quality checks and resolve schema mismatches.
  - ❌ Impute missing values (missing review ratings, unfulfilled delivery dates).
  - ❌ Standardize date strings into datetime objects.
  - ❌ Derive binary target variable `churn` (`1` if `auto_renew == 0` or subscription expired; `0` otherwise).
  - ❌ Engineer predictive features (`total_lifetime_spend`, `order_frequency`, `avg_star_rating`, `delivery_delay_rate`, `days_since_last_order`).
- ❌ **Load (`load()`):** Write final cleaned DataFrame into columnar Parquet format (`data/processed/processed_churn.parquet`).
- ❌ Validate Parquet output integrity and schema.

### Task 3: Machine Learning Pipeline (PySpark)
- ❌ Initialize PySpark `SparkSession` in `notebooks/pyspark_ml.ipynb`.
- ❌ Ingest `data/processed/processed_churn.parquet` into PySpark DataFrame.
- ❌ **Data Preprocessing:**
  - ❌ Encode categorical variables using `StringIndexer` and `OneHotEncoder`.
  - ❌ Assemble feature vectors using `VectorAssembler`.
  - ❌ Scale features using `StandardScaler` (where applicable).
- ❌ Split dataset into Training (80%) and Test (20%) sets.
- ❌ **Model Building & Experimentation:**
  - ❌ Train Logistic Regression classifier.
  - ❌ Train Random Forest classifier.
  - ❌ Train Gradient-Boosted Trees (GBT) classifier.
- ❌ **Hyperparameter Tuning:** Tune models using `ParamGridBuilder` and `CrossValidator`.
- ❌ Select best performing model based on evaluation metrics.

### Task 4: Evaluation, Export & Submission
- ❌ **Model Evaluation Plots:**
  - ❌ Generate and save annotated Confusion Matrices (`outputs/figures/`).
  - ❌ Plot and save ROC and Precision-Recall Curves.
  - ❌ Extract and plot Feature Importance rankings.
- ❌ Verify both notebooks execute cleanly top-to-bottom without any errors.
- ❌ Export both notebooks (`etl_pipeline.ipynb`, `pyspark_ml.ipynb`) into HTML files (`outputs/html/`).
- ❌ Team Leader (Parth Parkhiya) submits HTML files before deadline.

### Task 5: Presentation Preparation (No Slides Live Demo)
- ❌ Rehearse live code navigation between members without slides.
- ❌ Verify git commit log proves sustained individual contributions across all 4 members.
- ❌ Practice explaining code design decisions, Parquet trade-offs, and PySpark pipeline choices.
- ❌ Prepare for live code modification / parameter tuning requests from examiners.

---








