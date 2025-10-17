# AWS-Data-Lake-Setup

## OBJECTIVES:

- Use Amazon S3 as the storage layer of a data lake.
- Organize data into layers (or zones) in Amazon S3.
- Configure an S3 event notification to invoke an AWS Lambda function.
- Create an Amazon EventBridge rule to invoke the Lambda function.

## Services used:

- AWS Lambda
- Amazon Simple Storage Service (Amazon S3)

## Step 1: Review S3 buckets for raw zone and consumption zone

<img width="464" height="160" alt="{472F58C1-A19A-436D-A4C2-DDBF8EB77E08}" src="https://github.com/user-attachments/assets/2617a5f4-59fa-47de-8971-5711e5d9965f" />

You can split your buckets as per needs. Technically, in this reference, I will split by 2 buckets

- Raw Zone bucket
- Consumer Zone bucket

### Raw Zone bucket:
The raw zone (also called the raw layer) stores data directly as it is ingested from source systems, preserving its original and unmodified format. This layer serves as an immutable copy of the source data and can contain a variety of structured, semi-structured, or unstructured data types — such as database exports, backups, logs, images, and files in formats like JSON, CSV, XML, or plain text.

### Consumer Zone bucket:
Once the raw data is collected, the next step is to transform it. This process can include combining data from multiple sources, cleaning, or converting it into a different file format. In this setup, the consumption zone S3 bucket represents the transformed layer where processed data is stored and made ready for analysis or downstream use.

## Step 2: Set up S3 event notifications and route them to EventBridge

### Task 2.1: Create S3 event notification

1. Choose your raw zone bucket
2. Goto Properties tab

<img width="1476" height="454" alt="{8822A79A-41AE-4439-9B2D-CEF05FF76A93}" src="https://github.com/user-attachments/assets/8bdf6583-109c-4019-a912-cb85f44090a7" />

3. Click on Create Event Notification

<img width="1222" height="414" alt="{26B6AC72-2E96-4CC0-87D2-9CC9F69A2306}" src="https://github.com/user-attachments/assets/acd5415d-2669-4499-b657-711de6ec13fa" />

4. Enter event name

<img width="1460" height="447" alt="{C0037919-3FEA-4B40-9FC3-DE432049048D}" src="https://github.com/user-attachments/assets/58ca6daf-f2f8-42c8-a507-597b605d9b1e" />

5. Choose Put

<img width="1813" height="635" alt="{0AADE179-4A25-4324-8AC7-742DEC4F7B16}" src="https://github.com/user-attachments/assets/3db50ba7-d184-4502-ac7f-c1baaff119db" />

6. Choose destination Lambda Function
7. Under Specify Lambda function, you will need to choose anyone from the list
8. Save your changes

### Task 2.2: Send events to Amazon EventBridge

Amazon S3 can automatically send event notifications to Amazon EventBridge whenever specific actions occur within a bucket. Unlike traditional event destinations, EventBridge receives all event types from S3 without requiring you to manually select which ones to deliver.

9. Goto event notification section and in Amazon EventBridge click Edit

<img width="1831" height="173" alt="{3F6390DF-6EA6-44E6-8EE8-5414F6B25C3B}" src="https://github.com/user-attachments/assets/9f7c5509-1422-4660-b6b8-69774bb5acdd" />

10. Turn EventBridge ON and save changes

<img width="1440" height="215" alt="{9F7F3797-C43C-4EFB-808E-43ABA8D887BC}" src="https://github.com/user-attachments/assets/372c0503-d0c6-437e-858a-da64ed1e7431" />

## Step 3: Review the ingestion layer of your data lake architecture

I will review the injestion layer of the data lake solution

### Task 3.1: Configure and test the labFunction-Data-Generator Lambda function

1. Goto Lambda

<img width="467" height="163" alt="{D5E69150-1D11-45CD-9451-8DB0616B14DB}" src="https://github.com/user-attachments/assets/e2dffd91-f8a2-4ac2-9ca6-a3c51962714f" />

2. In the functions section -> choose xxxxx-xxxx-generator -> you will see code tab









