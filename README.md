# 🎥 YouTube Trending Data Pipeline — Real-Time Streaming Analytics

## ⭐ Project Overview

The **YouTube Trending Automation Pipeline** automates the collection, cleaning, and analysis of **YouTube trending video data** across multiple countries using **Azure Databricks** and **Azure Data Lake Storage**.  
It showcases a full **Data Engineering workflow** — from API ingestion to data transformation and analytics — orchestrated through **Databricks Jobs** and scheduled every **12 hours** for near real-time insights.

---

## 🧠 Objectives

- Automate data ingestion from **YouTube Data API** across multiple countries.  
- Clean and transform raw datasets using **PySpark**.  
- Perform advanced analytics with **SparkSQL**.  
- Store and manage data in **Azure Data Lake Gen2**.  
- Automate pipeline execution using **Databricks Job Scheduler**.

---

## 🏗️ Architecture Diagram

    ┌────────────────────────┐
    │  YouTube Data API v3   │
    └──────────┬─────────────┘
               │
               ▼
    ┌────────────────────────┐
    │  Databricks Notebook   │
    │   (DataFetch)          │
    └──────────┬─────────────┘
               │
               ▼
    ┌────────────────────────┐
    │  Data Cleaning &        │
    │ Transformation (PySpark)│
    │   (DataCleaning)        │
    └──────────┬─────────────┘
               │
               ▼
    ┌────────────────────────┐
    │  Data Analysis (SparkSQL)│
    │   (DataAnalysis)        │
    └──────────┬─────────────┘
               │
               ▼
    ┌────────────────────────┐
    │ Azure Data Lake Storage│
    │  source / transformation│
    │  / destination folders  │
    └────────────────────────┘

---

## 🧩 Azure Components Used

| Component | Purpose |
|------------|----------|
| **Azure Data Lake Gen2** | Data storage (raw → cleaned → analytics layers) |
| **Azure Databricks Community Edition** | Data transformation, scheduling, orchestration |
| **Databricks Jobs** | Task sequencing and scheduling every 12 hours |
| **YouTube Data API v3** | Real-time trending video metadata source |

---

## ⚙️ Pipeline Workflow

### **1️⃣ Data Ingestion — `DataFetch`**
- Fetches trending videos from **14+ countries** (IN, US, GB, JP, FR, DE, BR, CA, KR, RU, ZA, AU, MX, ID).  
- Retrieves video details:
  - `video_id`, `title`, `description`, `category`, `creator`, `views`, `likes`, `comments`, `hashtags`
- Supports **pagination** to fetch maximum possible results per region.
- Writes raw data to **Azure Data Lake → `source` container**.

📂 **Path:** `/source/youtube_trending_raw/`

---

### **2️⃣ Data Cleaning — `DataCleaning`**
- Removes duplicates and handles missing or invalid values.  
- Converts numeric fields to integers.  
- Adds derived columns:
  - `trending_date`
  - `engagement_rate = (likes + comments) / views`
- Stores cleaned dataset in **Azure Data Lake → `transformation` container**.

📂 **Path:** `/transformation/youtube_trending_cleaned/`

---

### **3️⃣ Data Analysis — `DataAnalysis`**
- Runs **SparkSQL** queries to generate insights:
  - Top trending categories by views.
  - Most engaging creators by engagement rate.
  - Trending hashtags by region.
- Saves results into **Azure Data Lake → `destination` container**.

📂 **Path:** `/destination/youtube_trending_analytics/`

---

## 🔁 Job Scheduling (Databricks)

| Task Name | Description | Trigger |
|------------|--------------|----------|
| **DataFetch** | Fetches new trending data | Every 12 hours |
| **DataCleaning** | Cleans and structures raw data | Triggered after DataFetch |
| **DataAnalysis** | Runs SparkSQL analytics | Triggered after DataCleaning |

📅 **Frequency:** Every 12 hours  
⚙️ **Execution Order:** `DataFetch → DataCleaning → DataAnalysis`  
📊 **Run Management:** Handled via Databricks Jobs UI with full lineage and logs.

---

## 🗂️ Azure Data Lake Folder Structure

| Folder | Purpose |
|--------|----------|
| **source/** | Raw data from YouTube API |
| **transformation/** | Cleaned and processed dataset |
| **destination/** | Aggregated analytics and insights |


---

## 🧰 Tech Stack

| Category | Tools / Services |
|-----------|------------------|
| **Language** | Python (Requests, Pandas, PySpark, SparkSQL) |
| **Cloud Platform** | Microsoft Azure |
| **Data Storage** | Azure Data Lake Gen2 |
| **Compute Engine** | Azure Databricks |
| **Source API** | YouTube Data API v3 |
| **Scheduling** | Databricks Job Scheduler (12-hour interval) |

---

## 📈 Sample Insights

🎶 **Most popular categories:** Entertainment, Music, and Games dominate globally.

🌍 **Top engagement rates:** Indian creators have the highest audience interaction.

🏷️ **Frequent hashtags:** #shorts and #viral lead the global trend.
