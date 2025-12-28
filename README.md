#  Feature Store and Data Versioning with Delta Lake

##  Overview

This project demonstrates how to build a **simple, versioned feature store** using **Delta Lake and PySpark**.  
It showcases how raw data can be ingested, transformed into **ML-ready features**, versioned reliably, and queried historically using **Delta Lake time travel**.

The goal is to ensure **reproducibility of machine learning experiments** by enabling access to **historical feature values**.

---

##  Key Concepts Demonstrated

- Raw data ingestion using **PySpark**
- Delta Lake tables with **transactional guarantees**
- Feature engineering and **feature store design**
- **Data versioning** using Delta Lake
- **Time travel queries** using:
  - `versionAsOf`
  - `timestampAsOf`

---

##  Technologies Used

- Python
- PySpark
- Delta Lake
- Apache Spark
- Jupyter Notebook
- WSL (Linux environment for Spark stability)

---

##  Project Structure

```text
feature-store-delta/
│
├── data/
│   └── raw/
│       └── user_events.csv
│
├── delta/                  # generated at runtime (not committed)
│   ├── bronze/
│   │   └── user_events/
│   └── features/
│       └── user_features/
│
├── notebooks/
│   ├── 01_ingestion.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_time_travel.ipynb
│
└── README.md
```

---

##  Phase 1: Raw Data Ingestion (Bronze Layer)

**Notebook:** `01_ingestion.ipynb`

- Raw CSV data is ingested using **PySpark**
- Data is written to a **Delta table (Bronze layer)**
- Delta transaction logs (`_delta_log`) are created automatically
- Table history is verified using:

```sql
DESCRIBE HISTORY delta.`path_to_bronze_table`
```

###  This ensures raw data is:
- Immutable
- Versioned
- Reproducible

---

##  Phase 2: Feature Engineering & Feature Store

**Notebook:** `02_feature_engineering.ipynb`

Derived **user-level features** are computed from the Bronze table:

| Feature | Description |
|------|------------|
| `total_events` | Total number of events per user |
| `total_clicks` | Number of click events per user |
| `avg_event_value` | Average event value per user |

These features are written to a **Delta Lake feature store**, where:

- Each update creates a **new version**
- Feature history is **preserved automatically**

---

##  Phase 3: Time Travel & Historical Feature Retrieval

**Notebook:** `03_time_travel.ipynb`

This phase demonstrates **Delta Lake time travel**, enabling access to **historical feature values**.

###  Version-Based Time Travel

```python
spark.read.format("delta") \
  .option("versionAsOf", 0) \
  .load(features_path)
```

###  Timestamp-Based Time Travel

```python
spark.read.format("delta") \
  .option("timestampAsOf", "YYYY-MM-DD HH:MM:SS.mmm") \
  .load(features_path)
```

### 🔍 Why this matters
- Reproducible ML training
- Debugging data drift
- Auditing feature changes

---

##  Key Outcomes

- Built a **mini feature store** using Delta Lake
- Implemented **data versioning** for raw and derived data
- Demonstrated **time travel queries**
- Followed **industry-standard ML engineering practices**

---

##  Why Delta Lake for Feature Stores?

- ACID transactions on data lakes
- Built-in versioning and audit history
- Native support for time travel
- Reliable and reproducible ML pipelines

---

## 🚀 How to Run the Project

1. Open the notebooks **in order**:
   - `01_ingestion.ipynb`
   - `02_feature_engineering.ipynb`
   - `03_time_travel.ipynb`

2. Restart kernel and run cells **top-to-bottom** in each notebook.

3. Ensure Spark is started with **Delta Lake support** in every notebook.

---

##  Conclusion

This project demonstrates a **practical and scalable approach** to building a **versioned feature store using Delta Lake**.  
It highlights how modern ML pipelines can maintain:

- Data consistency
- Traceability
- Reproducibility  

—all of which are essential for **production-grade machine learning systems**.

---


## Author

Guntur Ridhi

📧 gunturridhi@gmail.com

🔗 https://github.com/Ridhi-215
