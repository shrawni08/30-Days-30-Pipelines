# Day 4 - Boto3 S3 

## Project Overview

This project demonstrates how to use Python and Boto3 to perform common Amazon S3 operations.

The script acts as a small S3 automation toolkit and includes bucket management, file upload/download, multipart transfer, presigned URLs, object deletion, and permission management.


---

## AWS Services Used

* Amazon S3
* Boto3 (AWS SDK for Python)

---

## Features Implemented

### Bucket Operations

* Create S3 bucket
* List existing buckets
* Delete empty bucket
* Delete non-empty bucket

### File Operations

* Upload file to S3
* Upload file object
* Download file
* Download file object
* Delete S3 object

### Advanced S3 Operations

* Multipart upload configuration
* Concurrent download configuration
* Generate presigned download URL
* Generate presigned upload URL
* Change object permissions (ACL)

---

## Technologies Used

* Python
* AWS S3
* Boto3
* Requests
* VS Code
* Virtual Environment

---

## Setup Instructions

### 1. Create Virtual Environment

```powershell
python -m venv venv
```

### 2. Activate Virtual Environment

Windows PowerShell:

```powershell
.\venv\Scripts\Activate
```

### 3. Install Dependencies

```powershell
pip install -r requirements.txt
```

### 4. Configure AWS Credentials

Configure AWS CLI credentials before running the script.

```powershell
aws configure
```

### 5. Run the Script

```powershell
python s3_example.py
```

---

## Requirements

Dependencies are managed using:

```text
requirements.txt
```

Example packages:

* boto3
* requests
---

```

