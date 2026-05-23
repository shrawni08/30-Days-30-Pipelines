# Day 1 -- Data Cleansing Pipeline using AWS Glue

## Project Objective

This project demonstrates a basic ETL (Extract, Transform, Load)
pipeline using AWS Glue, Amazon S3, and AWS Glue Crawlers for data
cleansing and transformation.

The goal of this pipeline is to ingest raw data from an S3 bucket, use
AWS Glue to clean and transform the data, store the processed data in a
separate S3 location, and register the processed dataset for querying
and further analysis.

------------------------------------------------------------------------

## Pipeline Architecture

### Step 1: Create Source S3 Bucket

Upload raw data files into an Amazon S3 bucket.

### Step 2: Create Glue Crawler

Create an AWS Glue Crawler and point it to the source bucket.

-   Create or select a Glue Database
-   Do not manually provide a table name
-   Run the crawler

The crawler scans the source data and automatically creates metadata in
the Glue Data Catalog.

### Step 3: Review and Update Schema

After the crawler runs:

-   A table is created automatically inside the Glue Database
-   Review schema and column mappings
-   Use **Edit Schema** if corrections are needed

### Step 4: Create AWS Glue Visual ETL Job

Create a Visual ETL job in AWS Glue.

**Source Configuration** - Select S3 as source - Choose **Data Catalog
Table** because crawler has already populated the catalog

**Transformation** - Apply data cleansing or transformation logic

Examples: - Remove null values - Rename columns - Change data types -
Filter or map records

**Target Configuration** - Select S3 as destination - Store transformed
data in a separate processed-data bucket

Run the Glue job.

### Step 5: Create Second Glue Crawler

Create another crawler on the processed S3 location.

This crawler:

-   Infers schema of processed data
-   Creates catalog metadata
-   Registers transformed dataset for future use

------------------------------------------------------------------------

## What This Pipeline Is Actually Doing

Your understanding is mostly correct.

This pipeline performs a classic **ETL workflow**:

### Extract

Read raw files from source S3 bucket.

### Transform

Clean and modify the dataset using AWS Glue Visual ETL transformations.

### Load

Store transformed data into a new S3 location.

The second crawler plays an important role. It not only infers schema
but also registers the processed dataset in the AWS Glue Data Catalog,
making it available for Athena queries and downstream Glue jobs.

------------------------------------------------------------------------

## AWS Services Used

  Service                 Purpose
  ----------------------- -----------------------------------
  Amazon S3               Store raw and processed datasets
  AWS Glue Crawler        Schema discovery
  AWS Glue Data Catalog   Metadata management
  AWS Glue Visual ETL     Data transformation and cleansing

------------------------------------------------------------------------

## Learning Outcomes

By building this project you learn:

-   Creating S3 buckets
-   Using Glue Crawlers
-   Understanding Glue Data Catalog
-   Building Visual ETL pipelines
-   Performing data cleansing
-   Managing raw and processed datasets

------------------------------------------------------------------------



