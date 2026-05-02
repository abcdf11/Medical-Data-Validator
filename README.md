# 🏥 Healthcare Data QA Pipeline (ETL Validator)

## 🎓 Background & Inspiration
This project was built following the concepts introduced in the [freeCodeCamp "Build a Medical Data Validator" tutorial](https://www.youtube.com/watch?v=GetPPXviwUo). 

However, instead of validating hardcoded, perfectly clean Python lists (as done in the tutorial), I took the foundational logic and engineered it to process **real-world, messy CSV datasets**. By integrating **Pandas**, this script acts as a true Data Quality Assurance (QA) pipeline, anticipating and gracefully handling missing values, corrupted strings, and unexpected data types.

## 📌 Overview
In the healthcare and health-tech industries, raw data provided by hospital systems or legacy databases is frequently incomplete. If "dirty" data enters a production database, it can crash downstream analytics or cause billing systems to fail.

This Python script (`Medical Data Validator.py`) acts as the first line of defense in an Extract, Transform, Load (ETL) process. It ingests medical billing records, performs strict schema and content validation, and generates a detailed terminal audit report of any records that violate medical business logic.

## 📂 Data Source
The dataset used to test this pipeline (`medications.csv`) was generated using [**Synthea™**](https://synthetichealth.github.io/synthea/), an open-source synthetic patient population simulator. Using Synthea provides a highly realistic, complex healthcare dataset to test ETL logic while ensuring all patient records are 100% synthetic and HIPAA compliant.

## 🚀 Key Features

* **Schema Validation ("The Bouncer"):** Instantly rejects files that are missing required columns or contain unexpected "junk" columns before any processing occurs.
* **Content Validation ("The Rules Engine"):** Iterates through records applying strict domain constraints using defensive programming. 
  * Safely handles `NaN` values and empty strings.
  * Cleans and verifies financial data (e.g., stripping symbols and ensuring `BASE_COST` and `PAYER_COVERAGE` are non-negative numbers).
  * Enforces logical constraints (e.g., ensuring active prescriptions without a `STOP` date do not crash the parser).
* **Detailed Audit Reporting:** Instead of crashing when encountering bad data, the script logs exactly which rows failed and specifically which rules were violated.

## 📊 Sample Output

When running the pipeline against a dirty dataset(`medications_2.csv`), the terminal generates an easy-to-read audit report.In the example below, the script acts as a Bouncer, catching "junk" columns injected into the CSV and stopping the pipeline before corrupted data can be processed:
```text
Error: Invalid Format. Extra unknown columns detected: {'Unnamed: 13', 'adsasd'}
Pipeline stopped because the file format was invalid.
