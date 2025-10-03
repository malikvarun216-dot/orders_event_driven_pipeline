# 📦 Order Tracking Event Driven Data Ingestion  

## 📌 Overview  
This project demonstrates an **event-driven data ingestion pipeline** built on **Databricks** with **Delta Lake**.  
It automates ingestion of daily order tracking files, loads them into a staging Delta table, applies **SCD Type 1 merges** into the target table, and is orchestrated with **Databricks Workflows** triggered by file arrival.  

---

## ⚙️ Tech Stack  
- **Google Cloud Storage (GCS)** – Raw file storage  
- **Databricks Volumes** – Mounted storage layer  
- **PySpark** – Transformations & processing  
- **Delta Lake** – ACID-compliant data lake format  
- **Databricks Workflows** – Orchestration & event triggers  
- **GitHub** – Version control  

---

## 🚀 Workflow  

1. **File Arrival**  
   - New daily CSV file lands in Databricks Volume.  

2. **Stage Load** (`orders_stage_load.ipynb`)  
   - Reads CSV from `/Volumes/incremental_load/default/orders_data/source/`.  
   - Writes data to **staging Delta table**.  
   - Moves processed file to **archive folder**.  

3. **Target Load** (`orders_target_load.ipynb`)  
   - Performs **SCD Type 1 merge** from staging to target Delta table.  

4. **Automation**  
   - Orchestrated with **Databricks Workflows**.  
   - Multi-task job:  
     - `stage_load` → Staging  
     - `target_load` → Depends on staging success  
   - Trigger: **File arrival** at `/Volumes/incremental_load/default/orders_data/source/`.  

> ⚙️ Note: Workflow configuration can be exported as `workflow.json` directly from Databricks once the job is created. It is not included in this repo since it is unique to each workspace.  

---

## 📊 Data Flow  

```
GCS (Source) 
   ↓
Databricks Volume (Source Folder)
   ↓
orders_stage_load.ipynb ───▶ Delta Staging Table
   ↓                                │
Archive Folder                      ▼
                 orders_target_load.ipynb ───▶ Delta Target Table
```

---

## ✅ Features  
- Event-driven ingestion (file arrival trigger)  
- Automatic archival of raw files  
- SCD Type 1 merge strategy (latest state maintained)  
- End-to-end orchestration in Databricks  

---

## 🛠️ Getting Started  

### 1. Clone Repository  
```bash
git clone https://github.com/<your-username>/orders_event_driven_pipeline.git
cd orders_event_driven_pipeline
```

### 2. Import into Databricks  
- Navigate to **Repos** in Databricks.  
- Connect your GitHub account.  
- Import this repository.  

### 3. Run the Workflow  
- Create a Databricks Workflow (Job) from the UI.  
- Add `orders_stage_load.ipynb` and `orders_target_load.ipynb` as sequential tasks.  
- Set file arrival trigger on `/Volumes/incremental_load/default/orders_data/source/`.  
- Place a CSV file in the source folder to trigger the pipeline.  
