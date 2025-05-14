---
## 📌 Overview

This AWS Lambda function automates receipt processing using serverless components. It listens for receipt uploads to an S3 bucket, extracts structured data with Amazon Textract, stores the data in DynamoDB, and sends a summary email via Amazon SES.

---
> 📽️ [Watch Demo Video]
https://drive.google.com/file/d/1Ifse8lyQepcOBG2CWCR3Qbj8kF2N8HsD/view?usp=drive_link

Receipt Processing Diagram
https://i.ibb.co/Fb4L6Sm2/receipt-processing.png

## 📂 Code Structure

The Lambda function is logically divided into four main components:

1. Lambda Handler – Entry point that manages the entire workflow  
2. Textract Processing – Extracts structured data from receipt images  
3. DynamoDB Storage – Saves parsed data into a DynamoDB table  
4. Email Notification – Sends an email with a summary of the receipt  

---

## 🔍 Understanding the AWS Lambda Function Code

### 🧩 1. Lambda Handler Function

```python
def lambda_handler(event, context):
    ...
````

**Purpose:**

* Acts as the controller for the whole process
* Extracts and decodes file info from the S3 event
* Delegates tasks to specific helper functions
* Handles logging and error management

---

### 🧾 2. Textract Processing Function

```python
def process_receipt_with_textract(bucket, key):
    ...
```

**Key Functions:**

* Uses Textract’s `analyze_expense` API
* Extracts key fields: vendor, total, date
* Parses line items (name, price, quantity)
* Returns a normalized data object

---

### 🗃 3. DynamoDB Storage Function

```python
def store_receipt_in_dynamodb(receipt_data, bucket, key):
    ...
```

**What it Does:**

* Saves structured receipt data to DynamoDB
* Includes itemized purchases and processing timestamp
* Maintains a reference to the original S3 object

---

### 📧 4. Email Notification Function

```python
def send_email_notification(receipt_data):
    ...
```

**Purpose:**

* Composes a well-structured HTML email
* Includes receipt metadata and all line items
* Sends via AWS SES to predefined recipient(s)

---

## ⚙ Configuration: Environment Variables

| Variable              | Description                       |
| --------------------- | --------------------------------- |
| `DYNAMODB_TABLE`      | DynamoDB table name for storage   |
| `SES_SENDER_EMAIL`    | Verified sender email in SES      |
| `SES_RECIPIENT_EMAIL` | Recipient email for notifications |

---

## 🛠 Error Handling & Logging

* Robust `try/except` blocks used throughout
* Helpful error messages for debugging
* Default fallbacks for missing fields
* Partial failure tolerance (e.g., still stores data even if email fails)

---

## ✅ Summary

This Lambda system enables a seamless pipeline for automated receipt extraction, structured storage, and real-time notification, using the power of AWS Textract, DynamoDB, and SES.

---

```

---

