# Smart Image Recognition & Alert Pipeline

An event-driven AWS project that automatically detects objects in uploaded images using Amazon Rekognition and sends notifications through Amazon SNS.

## Project Overview

This pipeline uses AWS serverless services to identify objects inside images stored in Amazon S3.

When an image is uploaded (or analyzed through Lambda), Amazon Rekognition detects labels such as animals, vehicles, food, people, etc., and sends the result through SNS email notifications.

Example:

Image uploaded:

dog.jpg

Notification received:

Image Analyzed: dog.jpg

Detected Labels:
- Dog (99%)
- Mammal (98%)
- Pet (97%)

---

## Architecture

![Architecture](architecture.png)

---

## AWS Services Used

| Service | Purpose |
|----------|---------|
| Amazon S3 | Store image files |
| AWS Lambda | Trigger image analysis logic |
| Amazon Rekognition | Detect image labels |
| Amazon SNS | Send email notifications |
| Amazon CloudWatch | Monitor Lambda logs |

---

## Pipeline Flow

### Step 1 — Upload Image to S3

Image is uploaded into the S3 bucket.

Example:

```text
image-upload-rek/dog.jpg
```

---

### Step 2 — Lambda Execution

Lambda function is triggered (or manually tested).

Lambda:

- Reads bucket and image
- Calls Rekognition DetectLabels API

---

### Step 3 — Rekognition Image Analysis

Amazon Rekognition analyzes the image and returns detected labels with confidence scores.

Example:

```text
Dog – 99%
Pet – 98%
Mammal – 97%
```

---

### Step 4 — SNS Notification

Lambda formats the Rekognition output and publishes it to an SNS topic.

SNS sends email notification to subscribed users.

---

## Lambda Code

```python
import boto3

rekognition = boto3.client('rekognition')
sns = boto3.client('sns')

BUCKET_NAME = 'image-upload-rek'
IMAGE_NAME = 'dog.jpg'
TOPIC_ARN = 'YOUR_SNS_TOPIC_ARN'

def lambda_handler(event, context):

    response = rekognition.detect_labels(
        Image={
            'S3Object': {
                'Bucket': BUCKET_NAME,
                'Name': IMAGE_NAME
            }
        },
        MaxLabels=5,
        MinConfidence=80
    )

    labels = response['Labels']

    message = f"Image Analyzed: {IMAGE_NAME}\n\nDetected Labels:\n"

    for label in labels:
        message += (
            f"{label['Name']} "
            f"({round(label['Confidence'],2)}%)\n"
        )

    sns.publish(
        TopicArn=TOPIC_ARN,
        Subject='Image Detection Alert',
        Message=message
    )

    return {
        'statusCode': 200,
        'body': message
    }
```

---

## IAM Permissions Required

Lambda execution role requires:

- AmazonRekognitionReadOnlyAccess
- AmazonSNSFullAccess
- AmazonS3ReadOnlyAccess
- AWSLambdaBasicExecutionRole

---

## CloudWatch Monitoring

Lambda execution logs can be monitored in CloudWatch.

Useful for:

- Debugging errors
- Viewing Rekognition response
- Monitoring SNS execution

---

## Learning Outcomes

This project helped understand:

- Event-driven architecture
- AWS Lambda
- Amazon Rekognition APIs
- SNS notifications
- IAM permissions
- CloudWatch logging
- Serverless AWS workflows
