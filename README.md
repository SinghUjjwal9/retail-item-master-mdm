# **Retail Item Master Data Pipeline and Steward Review System**

## **1\. Introduction**

This project implements a complete Master Data Management (MDM) pipeline for retail item data coming from multiple stores.  
 The goal is to clean, standardize, cluster, and harmonize inconsistent store-level product records into a unified **Central Item Master**.

This system performs:

* Raw item ingestion

* Text standardization

* TF-IDF vectorization

* Product clustering

* Master creation

* Automatic item-to-master mapping

* Routing low-confidence matches to a review queue

* Manual approval workflow with Flask UI

* Daily incremental ingestion and auto-matching

---

## **2\. Key Features**

### **Raw Data Layer**

Stores unmodified store-level raw inputs in the `rawitems` table for auditability.

### **Standardization Layer**

Cleans names, brands, pack sizes, categories and generates normalized fields inside `itemmaster_initial`.

### **Clustering Engine**

Uses:

* TF-IDF vectorization

* Cosine similarity

* Agglomerative Clustering

* Brand and pack-size blocking

Outputs consolidated SKU groups into `central_item_master`.

### **Matching Engine**

`process_new_item()`:

* Cleans incoming item

* Scores similarity against master table

* Auto-maps high-confidence matches

* Pushes ambiguous cases to `review_queue`

### **Steward Review UI (Flask)**

Provides:

* Pending item list

* Detailed item \+ suggested master view

* Approve & Link

* Approve & Create

* Reject

Updates `itemmapping`, `central_item_master`, and `review_queue`.

### **Daily Automation**

`daily_ingest_and_match.py` automates:

1. Reading daily POS files

2. Appending new items to `rawitems`

3. Running matching engine

4. Auto-linking or routing to review

---

## **3\. Folder Structure**

`retail-item-master-mdm/`  
`│`  
`├── src/`  
`│   ├── 3_store_raw_data_generation.py`  
`│   ├── data_cleaning_raw_data.py`  
`│   ├── matching_engine.py`  
`│   ├── steward_ui.py`  
`│   └── daily_ingest_and_match.py`  
`│`  
`├── README.md`  
`├── requirements.txt`  
`├── LICENSE`  
`└── .gitignore`

---

## **4\. Technologies Used**

* Python

* Pandas

* NumPy

* scikit-learn

* SQLAlchemy

* PostgreSQL

* Flask

* Ngrok

---

## **5\. How to Run**

### **Run the core pipeline**

* Generate raw data

* Clean and prepare initial master

* Cluster and create central master

* Populate mapping and review queue

### **Start the Steward UI**

`python src/steward_ui.py`

A public Ngrok URL will appear.

### **Process Daily Files**

`export DAILY_NEW_ITEMS_FILE=/path/to/file.csv`  
`python src/daily_ingest_and_match.py`

---

## **6\. Automation Options**

### **Cron**

`0 2 * * * python /opt/retail-item-master-mdm/src/daily_ingest_and_match.py`

---

## **7\. Summary**

This project demonstrates a complete production-grade item master management pipeline including ingestion, cleaning, clustering, matching, stewardship, and automated daily updates.  
 It is suitable for real retail POS data standardization environments.

---

