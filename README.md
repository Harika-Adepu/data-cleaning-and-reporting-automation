# Automated Data Cleaning & Reporting Pipeline

An end-to-end **ETL (Extract, Transform, Load) and Business Intelligence (BI)** automation pipeline. Built completely in Python, this project eliminates manual data entry and spreadsheet tinkering by programmatically streaming messy raw web data, executing deep cleaning steps, resolving missing values, and instantly generating an executive visual report.

---

## 📌 Project Overview & Architecture

In production environments, raw incoming data is inherently messy. It frequently contains overlapping entries, broken text encodings, inconsistent schemas, and massive structural data gaps. Handling these manually in Excel sheets wastes valuable engineering time and introduces severe human error.

This project solves that exact problem by creating a completely hands-free automation pipeline that processes data through three distinct architectural phases:



1. **Extraction (Ingestion):** Streams the raw dataset dynamically over the network directly from a remote GitHub server repository.
2. **Transformation (Cleaning):** Enforces corporate data standardization rules—targeting structural duplication, data casing inconsistencies, corrupted string characters, and missing financial metrics.
3. **Loading (Reporting):** Automatically compiles and saves a high-density, perfectly clean CSV dataset, while spinning up a publication-ready, two-panel executive visual dashboard.

---

## 🛠️ The Data Cleaning Strategy (Step-by-Step)

The core pipeline engine inside `Automation.ipynb` executes five distinct data preprocessing transformations automatically whenever it runs:

### 1. Schema Standardization
The raw dataset contains messy column names with varying text formats (such as `DIRECTOR_facebook_likes` mixed with `Cast_Total_facebook_likes`). The pipeline strips out accidental surrounding whitespaces and converts all column names to a standard, uniform **UPPERCASE** layout. It also safely drops the completely redundant `TITLE_YEAR.1` column to save computing memory.

### 2. Row Integrity & Deduplication
To prevent duplicate records from inflating business statistics or biasing downstream charts, the pipeline runs a targeted duplicate filter. It checks for identical records based on a compound business key: `MOVIE_TITLE` + `TITLE_YEAR`. It keeps the first unique record and removes any overlapping copies.

### 3. Text Encoding & String Repair
Because the raw data was scraped from the web, it carries ugly encoding artifacts like trailing `?ÿ` bytes and hidden `\xa0` space blocks. The pipeline uses exact string replacement logic to trim away these corrupt anomalies, turning `Avatar?ÿ` into a perfectly pristine string: `Avatar`.

### 4. Intelligent Missing Value Imputation
Dropping rows that contain blank cells causes severe data loss. Instead, this pipeline heals data gaps using custom statistical rules based on data distributions:
* **Median Imputation:** For highly skewed numerical features—such as financial numbers (`GROSS`, `BUDGET`) and interaction metrics (`NUM_VOTED_USERS`, `DIRECTOR_FACEBOOK_LIKES`)—the pipeline calculates and inserts the **median** value of that column. The median is chosen over the mean because it is robust against extreme outliers.
* **Mode Imputation:** For the chronological `TITLE_YEAR` column, it automatically fills missing gaps with the **mode** (the most frequently occurring year), preserving the historical distribution before safely casting the feature to an integer type.

---

## 📊 Automated Executive Reporting

Once the data is 100% clean, the pipeline shifts to the automated BI layer. It initializes a high-resolution double-panel dashboard graphic (`automated_summary_report.png`) to immediately deliver business intelligence to stakeholders:

* **Panel 1: Financial Performance Analysis:** Automatically sorts the clean dataset to isolate and display a horizontal bar chart of the top 5 highest-grossing films, complete with clean labels.
* **Panel 2: Metric Interaction Matrix:** Automatically computes a correlation heatmap across key numerical variables. This lets analysts instantly understand how social media metrics (like Director Facebook Likes or Total Cast Likes) interact with the final `IMDB_SCORE`.

---

📈 Pipeline Performance Metrics
Upon executing end-to-end, the automation pipeline prints a clean operational report directly to the console console:

Data Density Achieved: 100% complete across all cells (Exactly 0.00% remaining missing values).

Row Integrity: Maintained 13 unique data profiles after dropping duplicate records.

Automation Level: Fully automated. The pipeline requires zero human oversight to download, clean, save, and visualize data from scratch.


## 📂 Project Structure & Deliverables

Running the pipeline automatically generates a structured workspace folder:

```text
├── Automation.ipynb            # The core Python automation pipeline notebook
├── automated_outputs/          # The deployment folder created by the script
│   ├── clean_data.csv    # Flawless, fully imputed output dataset (0% nulls)
│   └── automated_summary_report.png  # Publication-ready executive visual summary
└── unclean_data.csv
