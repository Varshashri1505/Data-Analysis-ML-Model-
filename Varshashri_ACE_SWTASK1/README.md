# Task 1 – IoT Data Ingestion into PostgreSQL & MongoDB

### Author
**Varsha Shri**  
ACE College of Engineering – Department of AI & Data Science  

---

## Goal
This task demonstrates how to **read an IoT dataset (verticals AQ, WF, SL)** from a CSV file and insert it into:
1. **PostgreSQL** – a relational SQL database  
2. **MongoDB** – a NoSQL document database  

You will verify that data has been inserted successfully in both databases.

---

## Dataset
- **File name:** `iot_dataset.csv`  
- **Example structure:**

| id | vertical | timestamp          | value1 | value2 | … | value12 |
|----|-----------|--------------------|--------|--------|---|---------|
| 1  | AQ        | 2025-10-26 10:00  | 15     | 18     | … | 45      |

---

## Requirements

Install these Python libraries first:

```bash
pip install pandas psycopg2-binary pymongo
```

Also ensure:
- **PostgreSQL** is installed and running  
- **MongoDB** is installed and running (use MongoDB Compass to view data)  
- Your CSV file is in the same folder as the Python script  

---

## Project Structure

```
Task1/
├── iot_dataset.csv
├── task1_insert_data.py
├── README.md
└── report_Task1.pdf
```

---

## Steps to Run

### Read the Dataset
Use **pandas** to load the CSV:

```python
import pandas as pd
df = pd.read_csv("iot_dataset.csv")
print("Dataset loaded successfully!")
print(df.head())
```

---

### Insert into PostgreSQL

1. Create database:

```sql
CREATE DATABASE iot_project;
```

2. Create table:

```sql
CREATE TABLE iot_data (
  id SERIAL PRIMARY KEY,
  vertical VARCHAR(10),
  timestamp TIMESTAMP,
  value1 FLOAT,
  value2 FLOAT,
  value3 FLOAT,
  value4 FLOAT,
  value5 FLOAT,
  value6 FLOAT,
  value7 FLOAT,
  value8 FLOAT,
  value9 FLOAT,
  value10 FLOAT,
  value11 FLOAT,
  value12 FLOAT
);
```

3. Run the Python insertion code (update your DB password as needed):

```bash
python task1_insert_data.py
```

Expected output:

```
Dataset loaded successfully!
Data successfully inserted into PostgreSQL!
```

---

### Insert into MongoDB

MongoDB connection:

```python
from pymongo import MongoClient
client = MongoClient("mongodb://localhost:27017/")
db = client["iot_project"]
collection = db["iot_data"]
collection.insert_many(df.to_dict("records"))
print("✅ Data successfully inserted into MongoDB!")
```

Expected output:

```
 Data successfully inserted into MongoDB!
```

---

##  Verification

### PostgreSQL (via pgAdmin / DBeaver)

```sql
SELECT COUNT(*) FROM iot_data;
SELECT * FROM iot_data LIMIT 5;
```

### MongoDB Compass / Shell

```bash
db.iot_data.countDocuments({})
db.iot_data.find().limit(5)
```

Check that record counts match the CSV rows.

---

## Summary of Workflow

| Step | Action | Tool/Library | Output |
|------|--------|--------------|---------|
| 1 | Load dataset | pandas | DataFrame preview |
| 2 | Insert into PostgreSQL | psycopg2 | “Data inserted successfully” |
| 3 | Insert into MongoDB | pymongo | “Data inserted successfully” |
| 4 | Verify data | pgAdmin + MongoDB Compass | View inserted records |

---

## **Environment Setup**
To run this project, ensure you have the following dependencies installed:

```bash
pip install pandas numpy scikit-learn xgboost matplotlib seaborn
```

---

## **Execution Steps**
1. **Load the datasets** (Air Quality, Water Flow, and Street Light).  
2. **Preprocess the data** – handle missing values, drop unnecessary columns, and clean data types.  
3. **Train baseline models** using:
   - Linear Regression  
   - Random Forest  
   - XGBoost  
   - Support Vector Regressor (SVR)  
4. **Evaluate performance** using R², RMSE, and MAE metrics.  
5. **Perform data augmentation** and **hyperparameter tuning** to enhance accuracy.  
6. **Visualize performance improvements** with bar plots for all metrics.

---

## **Modeling Approach**
### **1. Baseline Training (Task 2)**
Each dataset was trained with four regression models to predict the following targets:
- **Air Quality (AQ):** `calibrated_pm25`
- **Water Flow (WF):** `flowrate`
- **Street Light (SL):** `active_power`

Metrics such as **R²**, **RMSE**, and **MAE** were used to evaluate the models.

---

### **2. Improved Model Accuracy (Task 3)**
To enhance model performance, additional data was created using **synthetic augmentation** by adding small random noise to numeric features.  
Hyperparameter tuning using **GridSearchCV** optimized model parameters for:
- **Random Forest:** `n_estimators`, `max_depth`
- **XGBoost:** `n_estimators`, `learning_rate`

All models were retrained and re-evaluated on the expanded datasets.

---

## **Results and Evaluation**
- **XGBoost** and **Random Forest** achieved higher R² scores after tuning.  
- **Linear Regression** served as a simple baseline and underperformed in non-linear cases.  
- **SVR** performed moderately but less consistently compared to ensemble methods.  
- Visualizations of R², RMSE, and MAE confirmed the improvement post-tuning.

---

## **Visualizations**
Three bar plots were generated for performance comparison:
- **R² Score Comparison**
- **RMSE Comparison**
- **MAE Comparison**

These plots provided clear insight into which models performed best across each vertical.

---

## **Conclusion**
The project successfully demonstrates:
- Building regression models for multiple domains.  
- Improving accuracy with synthetic data and hyperparameter tuning.  
- Visualizing model performance effectively.  

**XGBoost** and **Random Forest** proved to be the most reliable models overall, showing the highest accuracy and robustness after optimization.


**Prepared by:** *Varsha Shri*  
*ACE College of Engineering – AI & Data Science*  
*November 2025*
