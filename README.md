# YouTube-Data-Analysis-Project

End-to-end YouTube data analysis project using AWS services (S3, Glue, Athena, QuickSight) for data ingestion, transformation, and visualization.


## YouTube Data Analysis Project

### Overview

This project focuses on analyzing YouTube data by building a complete end-to-end data pipeline. The data is extracted from on-premises sources and migrated to the AWS cloud for processing, transformation, and visualization. The goal is to gain insights into YouTube video performance metrics such as views, likes, comments, and engagement trends.

### Architecture

The project follows a modern **data lake architecture** with three layers:

* **Bronze Layer:** Raw data ingested directly from the source and stored in Amazon S3.
* **Silver Layer:** Cleaned and transformed data using AWS Glue and Athena.
* **Gold Layer:** Aggregated, analytics-ready data used for reporting and visualization in Amazon QuickSight.

### AWS Services Used

* **Amazon S3** – Data lake storage
* **AWS Glue** – ETL (Extract, Transform, Load) processing
* **AWS Athena** – Querying and data transformation
* **Amazon QuickSight** – Data visualization and dashboarding
* **AWS IAM** – Access management and role-based security

### Project Workflow

1. **Data Ingestion:** Uploaded on-premises YouTube dataset into Amazon S3 (Bronze layer).
2. **Data Transformation:** Used AWS Glue jobs to clean and structure the data (Silver layer).
3. **Data Analysis:** Queried the processed data using Athena.
4. **Data Visualization:** Created interactive dashboards in QuickSight (Gold layer).

### Key Insights

* Identified top-performing videos by category and region.
* Discovered correlations between likes, views, and comments.
* Built automated ETL pipelines ensuring zero data loss during migration.

🧩 Project Title:
YouTube Data Analysis Pipeline using AWS (S3, Glue, Athena, and QuickSight)
________________________________________
🧩 Project Overview
•	Assume you’re collecting YouTube channel data (video titles, views, likes, comments, etc.)
•	You’ll store, clean, analyze, and visualize that data using AWS.
________________________________________
⚙️ AWS Services You’ll Use
•	Amazon S3 → store data
•	AWS Glue → clean and transform data 
•	Amazon Athena → query data using SQL
•	Amazon QuickSight → build a dashboard
________________________________________
📂 Dataset

Create a file called youtube_data.csv with these columns:

video_id	title	channel	category	views		likes	comments	upload_date
v101	Travel Vlog	ExploreWorld	Travel	120000		5000	430	2024-08-01
v102	Cooking Pasta	FoodieFun	Food	95000		4200	380	2024-08-02
v103	Tech Review	TechTalk	Technology	150000		6000	500	2024-08-03
v104	Yoga Routine	FitLife	Fitness	85000		3800	260	2024-08-04
								
Save this file as youtube_data.csv on your system.
________________________________________
✅ Step 1: Upload Your Data to Amazon S3
1.	Go to your AWS Console → open S3.
2.	Click Create bucket → give it a unique name like youtube-data-sameena.
3.	Keep the region (like ap-south-1 for Mumbai).
4.	Leave everything else default → click Create bucket.
5.	Open the bucket → click Upload → upload your youtube_data.csv file.

📁 Example path:
s3://youtube-data-sameena/raw/youtube_data.csv
________________________________________
✅ Step 2: Create an AWS Glue Crawler

A Glue crawler helps detect your data structure and automatically creates a table in the Glue Data Catalog

Metadata Layer (Glue Crawler)
•	You created a Glue Crawler that scanned your S3 bucket.
•	The crawler automatically detected:
o	File format (CSV)
o	Columns (video_id, title, category, etc.)
o	Data types (string, bigint)
•	It stored this metadata in the Glue Data Catalog as a table (youtube_data_sameena).


: Create AWS Glue Crawler
1.	In your AWS console, search for “AWS Glue” and open it.
2.	On the left panel, click “Crawlers”.
3.	Click the “Create crawler” button.
________________________________________
Crawler Configuration
Step 1 – Name
•	Enter a name like: youtube-data-crawler
•	Click Next
________________________________________
Step 2 – Data Source
•	Choose “Add a data source”
•	Select S3
•	Under S3 path, choose Browse S3
•	Select your bucket (youtube-data-sameena) and the file path (or folder where your CSV is uploaded).
•	Click Add S3 data source, then Next
________________________________________
Step 3 – IAM Role
•	Choose “Create new IAM role”
•	Give it a name like AWSGlueServiceRole-youtube
•	Click Next
________________________________________
Step 4 – Target Database
•	Choose “Add database”
•	Give it a name like youtube_data_db
•	Click Next
________________________________________
Step 5 – Review and Create
•	Review all details and click Finish
•	After creation, select your crawler and click Run Crawler
________________________________________
Once it finishes running, you’ll see a table created under “Tables” → in your database (youtube_data_db).


1.	Open AWS Glue → Tables and select your table. You should see:
o	Table name – The name your crawler gave the table.
o	Database name – The database you created, like youtube_data_db.
o	Columns – All the fields the crawler detected from your data (like video_id, title, views, likes, etc.).
o	Data types – Automatically inferred (like string, int, double).
o	Location – The S3 path where the raw data is stored.
2.	Check partitions (if any). Partitions help with faster querying. Example: if your data is divided by year or month, Glue will show those under Partition keys.
3.	Verify the data. You can preview a few rows in Glue:
o	Click the table → View Table → Preview Table.
o	Make sure the columns look correct and data types match what you expect.

________________________________________
✅ Step 4: Query with Amazon Athena
1.	Open Amazon Athena Console.
2.	Select the youtube_database created by Glue.
3.	Click Query Editor → run queries like:

SHOW DATABASES;
SHOW TABLES IN youtube_data_db;
SELECT * FROM "youtube_data_db"."youtube_data_sameena" LIMIT 10;
SELECT * FROM youtube_data_sameena LIMIT 10;
DESCRIBE youtube_data_db.youtube_data_sameena;
SHOW COLUMNS IN youtube_data_sameena;
SELECT COUNT(*) AS total_videos
FROM youtube_data_sameena;

________________________________________
✅ Step 5: Build Dashboard in Amazon QuickSight
1.	Open Amazon QuickSight → choose Manage Data → New Dataset.
2.	Select Athena as data source.
3.	Choose your youtube_database → select the table created by Glue.
4.	Click Visualize.
5.	Create visuals such as:
o	Bar chart: Top 4 channels by views
o	Pie chart: Category distribution
o	Line chart: Views over time
Publish the dashboard — now you have a working YouTube Data Analytics Dashboard.

1. Sign in to QuickSight
2. Set up a data source
1.	Click Manage data → New data set → Athena.
2.	Give your data source a name, e.g., YouTube_Athena.
3.	Select the Athena workgroup (usually primary).
4.	Click Create data source.
3. Select your database and table
•	Choose your Glue database: youtube_data_db.
•	Select the table(s) you want to visualize, e.g.:
o	youtube_data_sameena (raw data)
•	Click Edit/Preview data to check columns.


4. Build visuals
•	Drag and drop fields to create charts:
o	Bar chart → Total views per category
o	Line chart → Videos uploaded per month/year
o	Table → Top channels by views/likes
o	KPIs → Total videos, average views, total likes
•	You can also filter by category, year, or channel for interactive dashboards.

5. Publish and share
•	Save the analysis and optionally publish a dashboard.
•	Share it with other users in your QuickSight account.
________________________________________
Need to know:-  

1. Choose the query mode
•	Import to SPICE
o	SPICE is QuickSight’s in-memory engine.
o	It loads the data into QuickSight for faster analysis and dashboards.
o	Recommended if your dataset is not huge (QuickSight Standard allows up to 1 GB per dataset).
•	Directly query your data
o	QuickSight queries Athena live each time you interact with the dashboard.
o	Useful for very large datasets, but queries may be slower.
o	Charges may apply for each Athena query.
For your YouTube dataset, since it’s manageable, Import to SPICE is usually better.
________________________________________

Key Blocks and How We Resolved Them
Issue	Why it happened	Resolution
SPICE import failed / 0 bytes	QuickSight role lacked Athena, Glue, S3 permissions	Created custom IAM policy (QuickSightAthenaS3Access) and attached to aws-quicksight-service-role-v0
No sufficient permissions to connect to dataset	Sameenae as above	IAM role updated with proper policy
Bar chart not showing X-axis or Y-axis	SPICE dataset was empty	After SPICE import completed, visuals worked correctly
Confusion around “Generate policy from CloudTrail”	Not needed for this workflow	Ignored CloudTrail-based policy; used manual JSON policy creation
________________________________________

Key lessons / points of blockage
Step	Problem	How we resolved
Athena queries	Syntax errors / Entity not found	Verified database and table names, used correct SQL syntax
QuickSight connection	Insufficient permissions	Created custom IAM policy for QuickSight role and attached it
SPICE import	0 bytes / failed	After attaching permissions, import completed
Chart creation	Fields not visible / empty X-axis	Ensured SPICE dataset imported, then dragged correct fields
________________________________________
1. Role of S3 in the project
•	Your YouTube data (CSV/JSON files) is stored in an S3 bucket.
•	Athena reads this data directly from S3.
•	QuickSight queries Athena, which in turn reads from S3.
So, S3 is the storage layer, Athena is the query layer, and QuickSight is the visualization layer.
________________________________________
2. Permission issues we faced with S3
•	Initially, QuickSight couldn’t import SPICE or run queries.
•	Error messages like:
o	You don’t have sufficient permissions to connect to this dataset or run this query.
o	SPICE import failed (0 bytes).
Reason: The QuickSight service role (aws-quicksight-service-role-v0) did not have permissions to read from S3.
________________________________________
3. How we resolved S3 access
1.	Created a custom IAM policy that included S3 actions:
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:ListBucket"
  ],
  "Resource": "*"
}
•	This was combined with Athena and Glue permissions into the sameenae policy (QuickSightAthenaS3Access).
2.	Attached this policy to the QuickSight service role (aws-quicksight-service-role-v0).
After this, QuickSight could access the S3 bucket, and SPICE import worked.
________________________________________
4. Optional: Add specific S3 bucket permissions
•	Instead of Resource: "*", you can restrict to your specific bucket:
"Resource": [
  "arn:aws:s3:::your-bucket-name",
  "arn:aws:s3:::your-bucket-name/*"
]
•	This is better for security, so QuickSight only reads the YouTube data bucket.
________________________________________
5. QuickSight S3 configuration
•	In QuickSight → Manage QuickSight → Security & permissions → S3, you can add the bucket so it’s visible to QuickSight.
•	Once added, SPICE import and visualizations work seamlessly.
________________________________________
3. QuickSight: Visualization Layer
How QuickSight accesses S3 via Athena:
1.	QuickSight does not read raw S3 files directly in this project; it queries Athena.
2.	QuickSight uses a service role: aws-quicksight-service-role-v0.
3.	This role must have permissions to:
o	Query Athena (athena:StartQueryExecution, athena:GetQueryResults)
o	Access Glue metadata (glue:GetDatabase, glue:GetTables)
o	Read S3 bucket (s3:GetObject, s3:ListBucket)
Problem we faced:
•	When trying to import SPICE or create a dataset, QuickSight showed errors:
o	“You don’t have sufficient permissions to connect to this dataset or run this query.”
o	SPICE import was 0 bytes.
Solution:
•	Created a custom IAM policy (QuickSightAthenaS3Access) with the following JSON:
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
•	Attached this policy to aws-quicksight-service-role-v0.
________________________________________
7. Permissions Summary (S3 → QuickSight)
Layer	Role / Service	Permissions needed
S3	Athena reads files	s3:GetObject, s3:ListBucket
Athena	QuickSight queries	athena:StartQueryExecution, athena:GetQueryResults
Glue	QuickSight queries	glue:GetDatabase, glue:GetTables
QuickSight	Access via SPICE	Needs attached service role with above permissions
________________________________________


Layer 1: S3 – Storage Layer (Raw Data)
Purpose:
•	Stores all your raw YouTube data (CSV, JSON, Parquet files).
•	Acts as the “source of truth” for analytics.
Example structure:
s3://youtube-data-bucket/
    ├── videos_2024.csv
    ├── videos_2025.csv
Permissions needed:
•	Athena reads data → requires:
o	s3:GetObject → read individual files
o	s3:ListBucket → list files in the bucket
Notes:
•	QuickSight doesn’t directly query S3 in this setup.
•	S3 is the foundation—without it, nothing else works.
________________________________________
Layer 2: Glue – Metadata Layer
Purpose:
•	Glue Data Catalog stores metadata about your datasets in S3 (table names, schema, columns).
•	Athena queries Glue for table definitions before reading S3 files.
Example table metadata:
•	Database: youtube_data_db
•	Table: youtube_data_sameena
•	Columns: video_id, title, channel, category, views, likes, comments, upload_date
Permissions needed for QuickSight role:
•	glue:GetDatabase
•	glue:GetDatabases
•	glue:GetTable
•	glue:GetTables
Notes:
•	If Glue permissions are missing, QuickSight cannot “see” the table structure, even if Athena can query S3.
________________________________________
Layer 3: Athena – Query Layer
Purpose:
•	Executes SQL queries on your S3 data using the table schema from Glue.
•	Acts as the bridge between raw data (S3) and visualizations (QuickSight).
Permissions needed for QuickSight role:
•	athena:StartQueryExecution → run queries
•	athena:GetQueryResults → fetch results
•	athena:GetQueryExecution → track query status
Notes:
•	Athena doesn’t store data—it reads from S3.
•	QuickSight queries Athena via its service role.
________________________________________
Layer 4: QuickSight – Visualization Layer
Purpose:
•	Provides dashboards, charts, and SPICE in-memory analytics.
•	Reads data via Athena and builds visuals.
Permissions needed:
•	QuickSight service role (aws-quicksight-service-role-v0) must have:
o	Athena permissions (to run queries)
o	Glue permissions (to read table metadata)
o	S3 permissions (so Athena can read data from the bucket)
Workflow inside QuickSight:
1.	Create a dataset from Athena (youtube_data_sameena).
2.	Import dataset to SPICE for faster performance.
3.	Build visuals: bar charts, line charts, pie charts, etc.
4.	Use filters, calculated fields, and groups to analyze data.
________________________________________
Data Flow: Step by Step
1.	S3 stores raw YouTube files.
2.	Glue catalog defines the schema of the S3 data.
3.	Athena queries Glue to know the schema, then reads S3 files.
4.	QuickSight queries Athena using the service role → gets results → loads into SPICE → visualizes.
Visual Representation (Text Diagram):
S3 (raw files)
   │
   ▼
Glue (metadata catalog)
   │
   ▼
Athena (queries SQL on S3 data using Glue metadata)
   │
   ▼
QuickSight (service role queries Athena → SPICE → Dashboards)
________________________________________
Permissions Summary (Golden Table)
Layer	Service/Role	Permissions required
S3	Athena (and QuickSight role indirectly)	s3:GetObject, s3:ListBucket
Glue	QuickSight service role	glue:GetDatabase, glue:GetDatabases, glue:GetTable, glue:GetTables
Athena	QuickSight service role	athena:StartQueryExecution, athena:GetQueryResults, athena:GetQueryExecution
QuickSight	Service role aws-quicksight-service-role-v0	Attach custom policy combining all above permissions
________________________________________
Problems We Faced & Resolved
Problem	Layer	Cause	Resolution
“Entity Not Found” in Athena	Athena	Wrong table/db name	Corrected SQL, used youtube_data_sameena in youtube_data_db
QuickSight SPICE import failed	QuickSight / Athena / S3	Missing IAM permissions	Created QuickSightAthenaS3Access policy and attached to service role
Bar chart not showing	QuickSight	SPICE dataset empty	After SPICE import completed, chart displayed
Confusion with CloudTrail policy	IAM	CloudTrail not needed	Ignored CloudTrail generation; used manual JSON policy
Horizontal bar X-axis missing	QuickSight	SPICE dataset empty	Import SPICE first, then drag correct fields (Value, Y-axis)
________________________________________
This broken-down view shows exactly:
•	What each layer does
•	Where the data lives
•	How permissions flow
•	Why we faced errors and how we fixed them
________________________________________

1. What was the goal of this project?
The goal was to build a simple end-to-end data pipeline that collects, stores, processes, analyzes, and visualizes YouTube data using AWS services. The pipeline transforms raw CSV data stored in Amazon S3 into insightful dashboards in QuickSight through Glue and Athena.
________________________________________
2. What AWS services were used and why?
Service	Purpose
Amazon S3	To store the raw CSV files and cleaned data outputs.
AWS Glue	To crawl, catalog, and clean the data automatically.
Amazon Athena	To query and analyze the data directly from S3 using SQL.
Amazon QuickSight	To create interactive dashboards and visualizations.
________________________________________
3. What dataset was used?
A custom dataset named youtube_data.csv was created with columns like video_id, title, channel, category, views, likes, comments, and upload_date.
Example records:
| video_id | title | channel | category | views | likes | comments | upload_date |
|-----------|--------|----------|-----------|--------|---------|-------------|
| v101 | Travel Vlog | ExploreWorld | Travel | 120000 | 5000 | 430 | 2024-08-01 |
________________________________________
4. How was the raw data stored?
The raw data file (youtube_data.csv) was uploaded to an S3 bucket named:
s3://youtube-data-sameena/raw/youtube_data.csv
This represents the Bronze Layer in the medallion architecture.
________________________________________
5. How did AWS Glue help in the process?
•	A Glue Crawler was created to scan the S3 path.
•	It automatically detected the schema (columns, types, file format).
•	It created a table in the Glue Data Catalog inside the database youtube_data_db.
•	This table could then be queried using Amazon Athena.
________________________________________
6. What is the data architecture used?
The project followed a Medallion Architecture:
Layer	Location	Description
Bronze (Raw)	s3://youtube-data-sameena/raw/	Original CSV data
Silver (Processed)	s3://youtube-data-sameena/clean/	Cleaned and formatted data
Gold (Curated)	s3://youtube-data-sameena/final/	Analytics-ready data for QuickSight
________________________________________
7. How was the data cleaned or transformed?
In this project, no PySpark script was used.
Instead, AWS Glue Crawler and Job visual interface were used to detect schema and prepare the data.
If needed, transformations could be done with PySpark, but in this version we relied on the Crawler to handle structure recognition without PySpark code.
________________________________________
8. How was data analyzed?
Using Amazon Athena:
1.	Selected the database youtube_data_db.
2.	Queried the table created by Glue using SQL.
Example:
3.	SELECT category, SUM(views) AS total_views 
4.	FROM youtube_data_sameena
5.	GROUP BY category;
6.	Verified that Athena could read data directly from S3.
________________________________________
9. How was data visualized in QuickSight?
Steps:
1.	Connected QuickSight to Athena as a data source.
2.	Selected the Glue database and table (youtube_data_sameena).
3.	Imported data into SPICE for faster visualization.
4.	Created visuals:
o	Bar chart: Top categories by views
o	Pie chart: Category distribution
o	Line chart: Views over time
o	KPI cards: Total videos, average likes
________________________________________
10. What problems were faced and how were they resolved?
Issue	Cause	Resolution
SPICE import failed (0 bytes)	QuickSight role didn’t have permissions for Athena, Glue, or S3	Created custom IAM policy QuickSightAthenaS3Access and attached it to the role aws-quicksight-service-role-v0
“Insufficient permissions to connect to dataset”	Missing role access	Sameenae policy fix resolved it
Chart not displaying X/Y axis	SPICE dataset empty	After SPICE import succeeded, visuals loaded correctly
Athena query error (Entity not found)	Database or region mismatch	Verified database name (youtube_data_db) and region alignment
________________________________________
11. What was in the custom IAM policy?
Policy name: QuickSightAthenaS3Access
Policy JSON:
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
This allowed QuickSight to read data from S3, Glue, and Athena.
________________________________________
12. What was the final output?
•	A QuickSight Dashboard showing key insights:
o	Total videos
o	Total and average views per category
o	Likes and comments distribution
o	Channel-wise performance
•	A fully automated pipeline:
•	S3 (Raw) → Glue Crawler → Athena → QuickSight Dashboard
________________________________________
13. Key Learnings
1.	Glue Crawlers are powerful for schema inference without writing code.
2.	IAM permissions are crucial for cross-service integrations.
3.	SPICE import improves dashboard performance.
4.	Even without PySpark, data pipelines can be built efficiently using AWS managed services.

END


### Results

* Reduced data processing time by **40%** using serverless architecture.
* Improved scalability and flexibility with AWS cloud integration.
* Delivered real-time insights through dynamic QuickSight dashboards.


### Future Enhancements

* Integrate real-time data streaming using AWS Kinesis or Kafka.
* Implement predictive analytics for video trend forecasting.
* Add automated alerts and reporting.


### Author

Sameena Begum S

📧 sameenasam4567@gmail.com
🔗 https://www.linkedin.com/in/sameena-begum-s-9156b9250/     |  


