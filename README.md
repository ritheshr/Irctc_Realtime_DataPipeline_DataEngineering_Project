# Irctc_RealTime_DataPipeline_DataEngineering_Project
IRCTC railways Data Engineering Project Using Pub/Sub, BigQuery, DataFlow
#Overview

This project implements a real-time data pipeline for IRCTC (Indian Railway Catering and Tourism Corporation) railway data using Google Cloud Platform (GCP) services. The pipeline ingests, processes, and visualizes railway booking data using Pub/Sub, DataFlow, and BigQuery.

##Architecture

#Data Generation & Ingestion:

Synthetic IRCTC booking data is generated and published to a Pub/Sub topic.

#Data Processing (ETL) with DataFlow:

A DataFlow job consumes Pub/Sub messages, transforms the data, and loads it into BigQuery.

#Data Storage & Analytics:

Processed data is stored in BigQuery, enabling analytics and visualization.

#Data Visualization:

The data is visualized using Looker Studio (formerly Data Studio).

##Technologies Used

Google Cloud Pub/Sub - Message queue for real-time ingestion.

Google Cloud DataFlow - Serverless data processing using Apache Beam.

Google BigQuery - Data warehouse for analytics.

Looker Studio - For visualization and reporting.

Python - For data generation and transformation.

##Project Components

#1. Data Generation (irctc_mock_data_to_pubsub.py)

This script generates mock IRCTC booking data and publishes it to a Pub/Sub topic.

Generates random user booking records.

Publishes the records as messages to Pub/Sub.

#2. Data Transformation (transform_udf.py)

#A User Defined Function (UDF) used in DataFlow to:

Clean and validate data fields.

Standardize and enrich user information.

Compute additional fields such as account age.

#3. Data Visualization (IRCTC_DATA_VISUAL.pdf)

A Looker Studio dashboard is used to visualize processed data, showing:

User demographics and account details.

Active users and their booking patterns.

Loyalty points distribution and trends.

Setup & Execution

#1. Prerequisites

Ensure you have the following:

A Google Cloud Project with billing enabled.

Pub/Sub, DataFlow, and BigQuery APIs enabled.

Python 3.x installed.

Google Cloud SDK installed and configured.

#2. Configure Pub/Sub

Create a Pub/Sub topic and subscription:
export PROJECT_ID=<your-gcp-project>
gcloud pubsub topics create irctc-data
gcloud pubsub subscriptions create irctc-sub --topic=irctc-data

#3. Run Data Generator

Publish mock data to Pub/Sub:
python irctc_mock_data_to_pubsub.py

#4. Deploy DataFlow Job

#Run a DataFlow pipeline to process messages:
gcloud dataflow jobs run irctc-dataflow-job \
    --gcs-location gs://dataflow-templates/latest/PubSub_to_BigQuery \
    --parameters \
        inputTopic=projects/$PROJECT_ID/topics/irctc-data,\
        outputTable=$PROJECT_ID:irctc_dataset.irctc_table,\
        javascriptTextTransformGcsPath=gs://your-bucket/transform_udf.js,\
        javascriptTextTransformFunctionName=transform_data

##5. Query Data in BigQuery

After processing, query the data in BigQuery:
CREATE TABLE `irctc_dwh.irctc_stream_tb` (
  row_key STRING,
  name STRING,
  age INT64,
  email STRING,
  join_date DATE,
  last_login TIMESTAMP,
  loyalty_points INT64,
  account_balance FLOAT64,
  is_active BOOL,
  inserted_at TIMESTAMP,
  updated_at TIMESTAMP,
  loyalty_status STRING,
  account_age_days INT64
);


##6. Visualize Data in Looker Studio

Connect BigQuery to Looker Studio to create dashboards

##Future Enhancements

Integrate Real IRCTC API for live data streaming.

Enhance Data Pipeline with AI/ML-based fraud detection.

Optimize Performance using DataFlow scaling strategies.
