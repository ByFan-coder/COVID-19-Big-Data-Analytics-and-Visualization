# Big Data COVID-19 Analytics Project

## Contributers：

**Weiyi Luo (WL3398)**

**Jia Yang (JY5081)**

**Geng Niu (GN2279)**

This repository contains the full implementation of our end-to-end big data pipeline for analyzing the global progression of COVID-19 using the OWID dataset (379k+ rows).  

Our workflow integrates **Hadoop HDFS**, **Hive (ODS → DWD → DWS)**, **Spark**, and **Python visualization**.

This README provides:

1. Instructions on how to run the code  
2. A high-level overview of the system logic  
3. A description of all files in the repository  
4. Project repository link  

---

## 1. Code Execution Instructions

Please following "HDFS and HIVE operations.pdf" to finish HDFS and Hive operations.
### **Step 1 — Upload dataset to HDFS**

```bash
hdfs dfs -mkdir /covid
hdfs dfs -put owid-covid-data.csv /covid/
```

### **Step 2 — Run Hive scripts to build the warehouse**

Open Hive CLI and execute:

#### **(1) Create ODS table (raw data)**

- Load raw CSV from HDFS
- All fields stored as **STRING**
- No cleaning at this stage

#### **(2) Create DWD table (cleaned data)**

- CAST all numeric fields from STRING → DOUBLE
- Standardize the date field
- Drop rows with missing country/continent
- Add year and continent partitions
- Store table in **Parquet format** for performance

#### **(3) Create DWS analytical tables**

Hive generates four summary tables:

- **dws_time_series_trend**
- **dws_country_risk_profile**
- **dws_vaccine_testing_effect**
- **dws_policy_effectiveness**

Each DWS table is exported to CSV:

```
INSERT OVERWRITE DIRECTORY 'hdfs:///exports/dws_xxx'
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
SELECT * FROM dws_xxx;
```

### **Step 3 — Download exported CSV files**

```
hdfs dfs -get /exports .
```

### **Step 4 — Run Spark notebooks**

Run the following locally or on a cluster:

- Spark_pro.ipynb — visualizations & analysis
- Bigdata_HDFS_Hive.ipynb — Hive-based queries + figures

These scripts automatically load DWS CSV files and generate plots for all eight research questions.



## ** 2. High-Level System Logic (Conceptual Overview)**

Our project follows a structured big data architecture:

### **(1) Data Ingestion**

- Upload OWID CSV to **HDFS**
- Create Hive **ODS** table pointing to raw files

### **(2) Data Cleaning & Processing — DWD Layer**

- Convert column types
- Normalize formatting
- Remove erroneous rows
- Partition by continent + year
- Store as Parquet for efficient querying

### **(3) Analytical Modeling — DWS Layer**

The DWS layer contains four prepared datasets used to answer all eight RQs:

1. **dws_time_series_trend**
   - Continent-level daily metrics
   - Includes rolling averages
2. **dws_country_risk_profile**
   - Infection & mortality rates
   - GDP, population, age, etc.
3. **dws_vaccine_testing_effect**
   - Vaccination coverage
   - Testing intensity
   - Positivity rate
4. **dws_policy_effectiveness**
   - Stringency index
   - Lagged variables for policy impact

### **(4) Spark Analytics & Visualization**

- Load DWS datasets into Spark
- Perform filtering, grouping, and time-series analysis
- Convert to pandas for plotting
- Produce visualizations for RQ1–RQ8

### **(5) Final Outputs**

- Figures for each research question
- Used in the final report and presentation



## ** 3. Repository Structure**

| **File**                       | **Description**                                        |
| ------------------------------ | ------------------------------------------------------ |
| Big Data Project Proposal.pdf  | Initial project proposal with eight research questions |
| BigDataFinal.pdf               | Final written report                                   |
| ppt.pptx                       | Final presentation slides                              |
| Bigdata_HDFS_Hive.ipynb        | Hive SQL operations + visualizations                   |
| Spark_pro.ipynb                | Spark-based analytics and Python plotting              |
| HDFS and HIVE operations.pdf   | Documentation of all Hive & HDFS operations            |
| dwd_covid_clean_full.csv       | Hive DWD cleaned dataset                               |
| dws_time_series_trend.csv      | DWS table for RQ1 & RQ5                                |
| dws_country_risk_profile.csv   | DWS table for RQ2 & RQ7                                |
| dws_vaccine_testing_effect.csv | DWS table for RQ3 & RQ6                                |
| dws_policy_effectiveness.csv   | DWS table for RQ8                                      |
| README.md                      | This file                                              |



## ** 4. Project Repository Link**

👉 **GitHub Repository:** https://github.com/ByFan-coder/COVID-19-Big-Data-Analytics-and-Visualization



## ** 5. Contributors**

- ByFan-coder
- Max-CCpersonal / Max-GengNIU
- Yolanda2002
