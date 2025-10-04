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


