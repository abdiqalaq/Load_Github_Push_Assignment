# Loading - Full ETL but continuation of ET

## 1. Project Overview
This project is an **ETL (Extract, Transform, Load) pipeline** built to demonstrate data extraction, cleaning, transformation, and preparation for further analysis.  
The main objective is to evaluate mastery in handling realistically sized datasets (8,000 – 10,000 rows), applying multiple transformations across different categories, and documenting every step of the ETL process in a reproducible manner.

The dataset used in this project is an **Online Retail transactional dataset**, which includes information about invoices, products, customers, quantities, and unit prices. The project emphasizes best practices in data quality assessment, handling missing values, removing duplicates, standardizing data types, enriching datasets with derived columns, and preparing both full and incremental datasets for analysis or reporting purposes.

---

## 2. Data Source
**Dataset:** [Online Retail Dataset](https://archive.ics.uci.edu/ml/datasets/online+retail) (UCI Machine Learning Repository)

**Description:**  
The dataset contains transactional data for a UK-based online retail store. Key columns include:

- `InvoiceNo`: Invoice number (unique transaction identifier)  
- `StockCode`: Product/item code  
- `Description`: Product description  
- `Quantity`: Number of units sold per transaction  
- `InvoiceDate`: Date and time of invoice  
- `UnitPrice`: Price per unit  
- `CustomerID`: Unique customer identifier  
- `Country`: Country of customer  

The dataset is suitable for ETL exercises because it contains common data quality issues such as missing values, duplicate rows, inconsistent data types, and opportunities for derived metric creation.

---

## 3. ET Phases

### Extract Phase
- **Loading Data:** Loaded `raw_data.csv` and created a smaller incremental subset `incremental_data.csv` for recent transactions.
- **Data Inspection:** Used `.head()`, `.info()`, and `.describe()` to explore the dataset structure, data types, and summary statistics.
- **Data Quality Assessment:** Identified key issues:
  1. Missing values in `CustomerID` and `Description`.  
  2. Duplicate transactions.  
  3. Inconsistent data types (e.g., `InvoiceDate` stored as object instead of datetime).
- **Data Merging:** Merged the incremental dataset into the main dataset using `pd.concat()` to ensure all records are included. Removed duplicates to maintain data integrity.
- **Validation:** Saved validated copies as `raw_validated.csv` and `combined_validated.csv` in the `/data/` folder.

### Transform Phase
- **Cleaning:**
  - Handled missing values by dropping rows with missing `CustomerID`.
  - Removed duplicate records to ensure unique transactions.
- **Standardization:**
  - Converted `InvoiceDate` to proper datetime format.
  - Ensured numeric columns (`Quantity`, `UnitPrice`) had correct types.
- **Enrichment:**
  - Created a derived column `TotalCost = Quantity * UnitPrice` to calculate total transaction value.
- **Structural Transformations:**
  - Filtered out invalid transactions with negative or zero values for `Quantity` or `UnitPrice`.
  - Categorized `TotalCost` into sales tiers (`Very Low`, `Low`, `Medium`, `High`, `Very High`) for analysis purposes.
- **Output:** Saved transformed datasets as `transformed_full.csv` and `transformed_incremental.csv` in the `/transformed/` folder.

---

## 4. Tools Used
- **Python 3.x** — programming language for scripting the ETL process.  
- **Pandas** — library for data manipulation and analysis.  
- **NumPy** — library for numerical operations.  
- **Jupyter Notebook / VS Code** — interactive environment for executing ETL notebooks.  
- **Git & GitHub** — version control and repository hosting.

---

## 5. Steps to Run the Project

### 1. Clone the Repository
```bash
git clone https://github.com/abdiqalaq/ET_Exam_Abdiqalaq_243.git
cd ET_Exam_Abdiqalaq_243



---

## Load & Verification

**Format Used:** SQLite

**Process:**
Data from `transformed_full.csv` and `transformed_incremental.csv` were loaded into a SQLite database file named `full_data.db` inside the `loaded/` folder using `sqlite3` and `pandas.to_sql()`.

**Verification:**
The following queries were executed to confirm successful loading and structure:
```python
pd.read_sql("SELECT * FROM full_data LIMIT 5", conn)
pd.read_sql("SELECT COUNT(*) AS count FROM full_data", conn)
