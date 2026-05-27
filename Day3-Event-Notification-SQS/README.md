# AWS S3 Event Notification with SQS

## Overview

This project demonstrates how to configure Amazon S3 Event Notifications to send object event messages to Amazon SQS.

Whenever a file is uploaded, deleted, or modified in the S3 bucket, Amazon S3 generates an event notification and sends it to an SQS queue. The queue stores these event messages, allowing downstream applications or services to process them asynchronously.

This setup is commonly used in event-driven and serverless architectures.

---

## Architecture

```
Object Upload/Delete in S3
            ↓
     S3 Event Notification
            ↓
        Amazon SQS Queue
            ↓
      Poll Messages in SQS
            ↓
   View Event Metadata Details
```

---

## AWS Services Used

- Amazon S3
- Amazon SQS
- IAM Policy (SQS Access Policy)

---

## Project Workflow

### Step 1 — Create S3 Bucket

Created an S3 bucket to store files and trigger event notifications.

Example:

```
s3-event-demo-bucket
```

---

### Step 2 — Configure S3 Event Notification

Inside the S3 bucket:

```
Properties → Event Notifications
```


Configuration:

- Event notification name
- Destination type:
<img width="1256" height="502" alt="Screenshot 2026-05-27 111212" src="https://github.com/user-attachments/assets/3b4edc1c-c30f-40a9-a0a0-ff1c0e471afc" />


```
SQS Queue
```

- Event types:

```text
All object create events
```

(or All operations depending on requirement)

This configuration enables S3 to automatically send event notifications to SQS.

---

### Step 3 — Create SQS Queue

Created an SQS queue to receive S3 event messages.

Example:

```text
s3-event-queue
```

The queue acts as a decoupled messaging layer between S3 and consumers.

---

### Step 4 — Configure SQS Queue Policy

Updated the SQS access policy to allow Amazon S3 to send messages to the queue.

Permission required:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "sqs:SendMessage"
      ],
      "Resource": "arn::DemoS3Notification"
    }
  ]
}
```

Without this policy, S3 cannot deliver event notifications to SQS.

---

### Step 5 — Test the Pipeline

Uploaded files into the S3 bucket.

Then checked:

```text
SQS → Poll for Messages
```

The queue received event messages containing object metadata.

---

## Sample SQS Event Message

Example message received:

```json
{
  "eventName": "ObjectCreated:Put",
  "s3": {
    "bucket": {
      "name": "demo-bucket"
    },
    "object": {
      "key": "sample-file.csv"
    }
  }
}
```

---

## What I Learned

Through this project I learned:

- How S3 Event Notifications work
- How to integrate S3 with SQS
- Importance of SQS access policies
- Event-driven architecture basics
- How AWS services communicate asynchronously

---

## Real-World Use Cases

This architecture is commonly used for:

- Triggering ETL pipelines
- Image or video processing workflows
- Log processing systems
- Serverless automation
- File ingestion pipelines
- Data lake event processing

---

## Future Enhancements

Possible improvements:

- Connect SQS to AWS Lambda
- Trigger Glue ETL jobs
- Add SNS notifications
- Build complete event-driven pipelines

---

