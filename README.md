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

## 👥 Team & Governance

> **Branching Policy:** Direct commits to `main` are restricted. All features are developed on individual branches (`dev_suraj`, `dev_parth`, `dev_dhruv`, `dev_hussnain`) and merged via peer-reviewed Pull Requests.

| Team Member | Student ID | Assigned Role | Core Responsibilities |
| :--- | :--- | :--- | :--- |
| **Suraj Chavan** | `26001944` | **Project Manager & Git Lead** | Repository architecture, branch protection, PR reviews, live demo coordination |
| **Parth Parkhiya** | `25107711` | **Data Engineer (Team Lead)** | Modular ETL (`extract`/`transform`/`load`), S3 ingestion, Parquet export, HTML submission |
| **Dhruv Patel** | `25117102` | **Data Scientist** | PySpark ML pipeline, feature vectorization, model training & hyperparameter tuning |
| **Hussnain Umer** | `25110803` | **Data Analyst** | Feature definitions, evaluation metrics, ROC/confusion plots, PR code reviews |

<details>
<summary><strong>📌 View Sprint & Deliverable Checklist</strong></summary>

- ✅ **Task 1 — Environment & Git Setup:** Configure repository structure, branch rules, and PR review workflow.
- ✅ **Task 2 — ETL Pipeline:** Implement `extract()`, `transform()`, and `load()` functions and export `processed_churn.parquet`.
- ✅ **Task 3 — Distributed ML:** Build PySpark preprocessing pipeline and train tuned LR, RF, and GBT classifiers.
- ✅ **Task 4 — Evaluation & Submission:** Export confusion matrices, ROC/PR plots (`outputs/figures/`), and HTML notebooks (`outputs/html/`).
- ❌ **Task 5 — Live Code Defense:** Rehearse slide-free code walkthrough and audit Git commit history across all contributors.

</details>

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
git clone https://github.com/parthHerts/P4G5_HERTS.git
cd P4G5_HERTS
```

## 2. Creating Virtual Enviornment for Notebooks:

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate

# Install required packages
python.exe -m pip install --upgrade pip
pip install -r requirements.txt  
python -m ipykernel install --user --name=p4g5-venv --display-name "Python (P4G5)"  # Select the P4G5 kernerl when opening notebook                
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
git checkout -b dev_suraj

# Parth (Data Engineer)
git checkout -b dev_parth

# Dhruv (Data Scientist)
git checkout -b dev_dhruv

# Hussnain (Data Analyst)
git checkout -b data-analyst-work
```

### 3. Commit Work Incrementally

Stage files and write clear, descriptive commit messages matching conventional commit guidelines:

```bash
git status
git add notebooks/etl_pipeline.ipynb data/processed/
git commit -m "feat(etl): add etl_pipeline and outputs"
```

### 4. Push Branch to GitHub

```bash
git push origin feature/etl-pipeline
```

### 5. Open Pull Request & Peer Review

1. Go to `https://github.com/parthHerts/P4G5_HERTS.git`.
2. Click **Compare & pull request**.
3. Set Title: `[ETL] Ingestion, Data Cleaning and Parquet Export`.
4. Assign at least one peer reviewer (e.g., Dhruv or Hussnain).
5. Once approved by the reviewer, merge into `main` and delete the feature branch locally:

   ```bash
   git checkout main
   git pull origin main
   git branch -d dev_suraj
   ```
---

## 📂 Project Structure

```text
P4G5_HERTS/
├── data/
│   ├── raw/                        # Immutable source SQLite DB and CSV logs
│   └── processed/                  # Cleaned feature store (processed_churn.parquet)
├── notebooks/
│   ├── etl_pipeline.ipynb          # Task 2: Python/Pandas ETL & data quality checks
│   └── pyspark_ml.ipynb            # Task 3: Distributed PySpark ML & CrossValidator
├── outputs/
│   ├── figures/                    # Confusion matrices, ROC/PR curves, feature importance
│   └── html/                       # Standalone HTML exports of executed notebooks
├── .env.example                    # Template for S3/MinIO environment variables
├── .gitignore                      # Excludes venv, .env, cache, and temp artifacts
├── requirements.txt                # Reproducible Python dependency list
└── README.md                       # Project documentation & live walkthrough guide
```

---
