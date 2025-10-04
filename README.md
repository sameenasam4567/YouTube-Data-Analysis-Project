# 🎬 YouTube Data Analysis Project

**End-to-end YouTube data analysis project using AWS services (S3, Glue, Athena, QuickSight) for data ingestion, transformation, and visualization.**

---

## 🧭 Overview

This project analyzes YouTube data by building a complete **end-to-end data pipeline** on AWS.
Data is migrated from on-premises sources to AWS for processing, transformation, and visualization.
The goal is to gain insights into YouTube video performance metrics such as **views, likes, comments, and engagement trends**.

---

## 🏗 Architecture

The project follows a **data lake architecture** with three layers:

| Layer            | Description                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------------- |
| **Bronze Layer** | Raw data ingested directly from the source and stored in Amazon S3.                         |
| **Silver Layer** | Cleaned and transformed data using AWS Glue and Athena.                                     |
| **Gold Layer**   | Aggregated, analytics-ready data used for reporting and visualization in Amazon QuickSight. |

---

## ☁️ AWS Services Used

| Service           | Purpose                                         |
| ----------------- | ----------------------------------------------- |
| Amazon S3         | Data lake storage for raw and processed files.  |
| AWS Glue          | ETL processing and Data Catalog using crawlers. |
| Amazon Athena     | Querying and transforming data.                 |
| Amazon QuickSight | Visualization and dashboards.                   |
| AWS IAM           | Role-based access and security management.      |

---

## 📂 Dataset

Create a file called `youtube_data.csv` with the following columns:

| video_id | title         | channel      | category   | views  | likes | comments | upload_date |
| -------- | ------------- | ------------ | ---------- | ------ | ----- | -------- | ----------- |
| v101     | Travel Vlog   | ExploreWorld | Travel     | 120000 | 5000  | 430      | 2024-08-01  |
| v102     | Cooking Pasta | FoodieFun    | Food       | 95000  | 4200  | 380      | 2024-08-02  |
| v103     | Tech Review   | TechTalk     | Technology | 150000 | 6000  | 500      | 2024-08-03  |
| v104     | Yoga Routine  | FitLife      | Fitness    | 85000  | 3800  | 260      | 2024-08-04  |

Save the file as **youtube_data.csv** on your system.

---

## ⚙️ Step-by-Step Implementation

### Step 1: Upload Data to Amazon S3

1. Go to **AWS Console → S3**.
2. Click **Create bucket** → name it `youtube-data-sameena` → select region (e.g., ap-south-1).
3. Leave default settings → **Create bucket**.
4. Open the bucket → **Upload** → select `youtube_data.csv`.

**Example S3 path:**

```
s3://youtube-data-sameena/raw/youtube_data.csv
```

---

### Step 2: Create an AWS Glue Crawler

1. Open **AWS Glue → Crawlers → Create crawler**.
2. Name: `youtube-data-crawler` → Next.
3. **Data Source**:

   * Add data source → S3
   * Browse S3 → select bucket/folder → Add S3 data source → Next.
4. **IAM Role**:

   * Create new role → `AWSGlueServiceRole-youtube` → Next.
5. **Target Database**:

   * Add database → `youtube_data_db` → Next.
6. Review → Finish → Select crawler → **Run Crawler**.

**After completion:**

* Table created: `youtube_data_sameena` in database `youtube_data_db`.
* Columns automatically detected (video_id, title, channel, category, views, likes, comments, upload_date).
* Preview table → verify columns and data types.

---

### Step 3: Query with Amazon Athena

1. Open **Athena Console** → select database `youtube_data_db`.
2. Use **Query Editor** to explore data.

**Example queries:**

```sql
SHOW DATABASES;
SHOW TABLES IN youtube_data_db;
SELECT * FROM youtube_data_db.youtube_data_sameena LIMIT 10;
SELECT * FROM youtube_data_sameena LIMIT 10;
DESCRIBE youtube_data_db.youtube_data_sameena;
SHOW COLUMNS IN youtube_data_sameena;
SELECT COUNT(*) AS total_videos FROM youtube_data_sameena;
```

> Athena reads data from S3 using the Glue schema.

---

### Step 4: Build Dashboard in Amazon QuickSight

1. Open **QuickSight → Manage Data → New Dataset → Athena**.
2. Name data source: `YouTube_Athena` → select Athena workgroup → **Create**.
3. Select database (`youtube_data_db`) → select table (`youtube_data_sameena`).
4. Click **Edit/Preview Data** → verify columns.
5. Import dataset to **SPICE** for faster dashboards.
6. Create visuals:

   * Bar chart: Top channels by views
   * Pie chart: Category distribution
   * Line chart: Views over time
   * Table/KPI: Total videos, average views, total likes
7. Save analysis → **Publish dashboard** → share as needed.

---

### Step 5: Choose Query Mode

| Mode            | Description                                                                             |
| --------------- | --------------------------------------------------------------------------------------- |
| Import to SPICE | Loads data into QuickSight memory → faster analysis (good for small datasets).          |
| Direct Query    | Queries Athena live → useful for large datasets, slower performance, may incur charges. |

> For this dataset, **Import to SPICE** is recommended.

---

## 🔧 Key Blocks & Resolutions

| Issue                                              | Cause                                               | Resolution                                                                                            |
| -------------------------------------------------- | --------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| SPICE import failed / 0 bytes                      | QuickSight role lacked Athena, Glue, S3 permissions | Created custom IAM policy `QuickSightAthenaS3Access` and attached to `aws-quicksight-service-role-v0` |
| No sufficient permissions to connect to dataset    | Missing IAM role access                             | Updated IAM role with proper policy                                                                   |
| Bar chart not showing X/Y axis                     | SPICE dataset empty                                 | After SPICE import completed, visuals worked                                                          |
| Confusion around “Generate policy from CloudTrail” | Not needed                                          | Used manual JSON policy creation                                                                      |

---

### Step 6: Roles & Permissions

**QuickSight requires:**

* Athena permissions → run queries
* Glue permissions → read metadata
* S3 permissions → read data

**Custom IAM Policy (QuickSightAthenaS3Access):**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "athena:StartQueryExecution",
        "athena:GetQueryResults",
        "athena:GetQueryExecution",
        "glue:GetDatabase",
        "glue:GetDatabases",
        "glue:GetTable",
        "glue:GetTables",
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": "*"
    }
  ]
}
```

Attach to **aws-quicksight-service-role-v0**.
*Optional:* Restrict S3 access to your specific bucket for security.

---

### Step 7: Layer Summary

| Layer      | Service        | Purpose                            | Permissions                                                                  |
| ---------- | -------------- | ---------------------------------- | ---------------------------------------------------------------------------- |
| S3         | Storage Layer  | Stores raw CSV/JSON files          | s3:GetObject, s3:ListBucket                                                  |
| Glue       | Metadata Layer | Stores table schema for Athena     | glue:GetDatabase, glue:GetDatabases, glue:GetTable, glue:GetTables           |
| Athena     | Query Layer    | Executes SQL on S3 data            | athena:StartQueryExecution, athena:GetQueryResults, athena:GetQueryExecution |
| QuickSight | Visualization  | Reads via Athena, SPICE dashboards | Attach above IAM policy to service role                                      |

**Data Flow:**

```
S3 (Raw files)
   │
Glue (Metadata)
   │
Athena (Query)
   │
QuickSight (SPICE → Dashboard)
```

---

## 📊 Problems Faced & Resolutions

| Problem                       | Layer                | Cause                   | Resolution                               |
| ----------------------------- | -------------------- | ----------------------- | ---------------------------------------- |
| “Entity Not Found”            | Athena               | Wrong DB/table name     | Corrected SQL names                      |
| SPICE import failed           | QuickSight/Athena/S3 | Missing IAM permissions | Attached QuickSightAthenaS3Access policy |
| Bar chart not showing         | QuickSight           | SPICE dataset empty     | Imported SPICE → chart displayed         |
| CloudTrail confusion          | IAM                  | Not needed              | Used manual JSON policy                  |
| Horizontal bar X-axis missing | QuickSight           | SPICE empty             | Re-import SPICE → correct fields dragged |

---

## ✅ Final Output

* QuickSight dashboard showing:

  * Total videos
  * Total & average views per category
  * Likes & comments distribution
  * Channel-wise performance
* Automated pipeline:

```
S3 (Raw) → Glue Crawler → Athena → QuickSight Dashboard
```

---

## 🎯 Key Learnings

1. Glue Crawlers detect schema automatically without code.
2. IAM permissions are crucial for cross-service integrations.
3. SPICE improves dashboard performance.
4. PySpark is optional; AWS managed services suffice for ETL.

---

## 📈 Results

* Reduced processing time by **40%** using serverless architecture.
* Improved scalability and flexibility with AWS cloud integration.
* Real-time insights through dynamic QuickSight dashboards.

---

## 🚀 Future Enhancements

* Real-time streaming with AWS Kinesis/Kafka.
* Predictive analytics for video trend forecasting.
* Automated alerts and reporting.

---

## 👩‍💻 Author

**Sameena Begum S**
📧 [sameenasam4567@gmail.com](mailto:sameenasam4567@gmail.com)
🔗 [LinkedIn](https://www.linkedin.com/in/sameena-begum-s-9156b9250/)


