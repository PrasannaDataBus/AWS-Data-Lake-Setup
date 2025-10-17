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
3. Review index.py script
4. After choose configuration tab

<img width="1520" height="227" alt="{791D72C1-323B-4F26-BDBA-AD04CE86130C}" src="https://github.com/user-attachments/assets/343346fd-6c61-4611-ad5c-c45697e6e33d" />

5. Click on environment variables

<img width="1536" height="206" alt="{2EA11A45-6D97-4DF6-8B78-F8C6C5FBE230}" src="https://github.com/user-attachments/assets/ec590fb1-3561-4197-83df-5d1a05e82ff5" />

6. You need to add or replace in environment variable and save

7. Choose code tab and test

<img width="454" height="594" alt="{B7EFFE2E-D331-42C2-A6F1-F2C946D52343}" src="https://github.com/user-attachments/assets/7441e6bd-9498-4cd4-970e-88b0356a8c88" />

8. Create or search your event

<img width="751" height="90" alt="{9A1699A6-40FF-477F-B895-2546C20AB769}" src="https://github.com/user-attachments/assets/f12be661-2f0d-4c28-bc9a-9eb71d3712dd" />

9. If created new event, configure and save ot

<img width="701" height="469" alt="{EA1047FF-E9F4-43F6-A26B-B1328DBF47A9}" src="https://github.com/user-attachments/assets/1f023390-5e0d-416e-8022-261f61d88610" />

10. Run the test event

**************************************** OUTPUT *************************************
Status: Succeeded
Test Event Name: TestEvent

Response:
null

Function Logs:
START RequestId: xxx-rrr-hhh-ooo-vvv Version: $LATEST
cart_id  customer_id  product_id  product_amount product_price
0        8            4           9              19         $3.43
1        1            4          10               7        $12.83
2        1            5           5               5        $75.79
3        9            0           8              10     $4,785.77
4        2            3           4               5       $861.11
END RequestId: safafaf-erwerwe-sdgdsgsg-34324-gfhtryt
REPORT RequestId: affewfew-FGDFGGERG-hhfdhf-asdasd-bdfgh	Duration: 5494.37 ms	Billed Duration: 7866 ms	Memory Size: 128 MB	Max Memory Used: 128 MB	Init Duration: 2371.63 ms

Request ID: sdfewfewf-sgdsggs-gsgsdg-wrrrer-DDSSDGDG

### Task 3.2: Review data in the raw zone S3 bucket

1. Goto S3 and you will see the raw injested data. Check the box in the objects tab
2. You will also be able to download the dataset to review

## Step 4: Review the processing layer for your data lake solution

After the raw data is ingested into the data lake, the processing layer is ready to start transforming the data and sending it to the consumption zone S3 bucket.

### Task 4.1: Configure the labFunction-Data-xxxxxx Lambda function

1. Goto lambda and chosoe the processor function

<img width="1232" height="439" alt="{58B47C09-6F34-4A5D-AE4C-AC91AEC86E00}" src="https://github.com/user-attachments/assets/36f65916-59fc-4d14-86db-181127324aca" />

2. In the Function overview section, review to ensure that S3 is listed as the function trigger.
3. Goto code tab
4. Then goto configuration tab and choose environment variables

<img width="1527" height="162" alt="{4092DA7A-B42F-4E1D-B99B-687DDC1C3FA6}" src="https://github.com/user-attachments/assets/85886973-38da-40bf-8c1e-c025f68759f7" />

5. Replace values in input and output bucket

6. Goto functions sections and choose generator

7. Click on test to see the transformation results

******************************* Output ****************************************

Status: Succeeded
Test Event Name: TestEvent

Response:
null

Function Logs:
START RequestId: dsgsdgsgs-dsfdsgdsg-zvzv-dsgss-xbbsbbs Version: $LATEST
cart_id  customer_id  product_id  product_amount product_price
0        3            7          10              15    $95,636.60
1        3            5           1              16        $62.33
2        6            0           3               1    $29,635.06
3        2            9           3              15    $49,297.85
4        4            8           9              14        $21.07
END RequestId: safafaf-xbfsbffsb-afafaf-b2\c\c\ec-vzvvdv
REPORT RequestId: bgngfngn-ngdndggngn-adfcaf-afafa-srggeee	Duration: 5580.44 ms	Billed Duration: 8128 ms	Memory Size: 128 MB	Max Memory Used: 128 MB	Init Duration: 2546.85 ms

Request ID: xvxvx-grgr-sdgsg-gsdsd-GSGGSSEREWR

### Task 4.2: Review data in the consumption zone S3 bucket

Goto S3, export to review the dataset

## STEP 5: Review Consumption layer of Data Lake

### Task 5.1: Create an Amazon EventBridge rule

<img width="717" height="208" alt="{2F1E4553-2DD8-4829-A107-2559E652BE49}" src="https://github.com/user-attachments/assets/7084722b-5194-4b24-a4d3-3475a91a314c" />

1. Click on create rule

<img width="1141" height="631" alt="{D05C38D7-5FBC-4537-8F6E-E43D539ECAAE}" src="https://github.com/user-attachments/assets/bc059f5b-fd68-422e-9719-88065d9e9e0b" />

2. Choose rule type and click next
3. Build event pattern

<img width="1154" height="545" alt="{3D3AD6BE-10BE-4B4F-A9E8-B95E23FF23FA}" src="https://github.com/user-attachments/assets/e07bc2cc-9822-4f2b-a061-6eab3ee9c9bf" />

4. Select event pattern

<img width="1124" height="674" alt="{1DA43920-AD11-458E-9B69-47BDCEB870A2}" src="https://github.com/user-attachments/assets/7f4c840b-ef9a-4890-a17e-56b87660deac" />

5. Event source = AWS Services, AWS Service = Simple Storage Service (S3), Event Type = Amazon S3 Event Notification and review the Event Pattern

<img width="1126" height="581" alt="{83248207-11E8-49DE-9EB2-40823AA46D1F}" src="https://github.com/user-attachments/assets/3378c075-9712-444d-8efb-3f6ca8697736" />


*********************************** OUTPUT *************************************


{
  "source": ["aws.s3"],
  "detail-type": ["Object Access Tier Changed", "Object ACL Updated", "Object Created", "Object Deleted", "Object Restore Completed", "Object Restore Expired", "Object Restore Initiated", "Object Storage Class Changed", "Object Tags Added", "Object Tags Deleted"]
}

6. Choose Event Type Specification 1 = Specific Event (select object created), Event Type Specification 2 = Specific buckets(s) by name (add your raw bucket name)

<img width="568" height="526" alt="{6E0D70DE-C7C3-41E6-9D42-547A4B6DD772}" src="https://github.com/user-attachments/assets/acb6f408-3d4c-4af8-9959-b5266e286218" />

7. Select target(s)

<img width="1128" height="771" alt="{0BD0E611-FA09-440C-85DD-56B67CC7B7C4}" src="https://github.com/user-attachments/assets/1a087f64-213a-4fcc-b62b-7d3812396829" />

8. Select target -> Lambda function, Function -> Choose function, Under Permissions uncheck Use execution role (recommended) and click next
9. Configure tags (optional)
10. Review and create, review event pattern

****************************** Example ******************************
{
  "source": ["aws.s3"],
  "detail-type": ["Object Created"],
  "detail": {
    "bucket": {
      "name": ["raw-bucket-rr-eee-1-dDaaaaaaaaaaa"]
    }
  }
}

11. Create rule

### Task 5.2: Configure the labFunction-Promotion-App function

1. Goto lambda and in functions sections choose the xxxxxx-prom-app
2. Review the function overview and confirm does EventBridge appears in trigger

<img width="1222" height="446" alt="{13A92EFE-A68C-40E3-9235-B1FC8A464309}" src="https://github.com/user-attachments/assets/8ba2ea76-d1ab-48c3-873e-586879e54896" />

3. Scroll down to code
4. Goto configuration tab and choose environment variables -> edit -> provdie input and output bucket details and click save
5. Goto functions and in code space test your event

********************************** Output *************************************
Status: Succeeded
Test Event Name: TestEvent

Response:
null

Function Logs:
START RequestId: fsdff-3434-dfffwfwee-fdzfsd-fsfssddsdsf Version: $LATEST
cart_id  customer_id  product_id  product_amount product_price
0        9            6           2               7       $936.75
1        4           10           0               1     $2,937.08
2        0            6          10              16        $21.47
3        0            8           1              14         $7.36
4        6            4           1              13    $22,136.00
END RequestId: fsfdsffdsdsfsd-3244141-faaa-fffdafaf-faffdffesewwe
REPORT RequestId: fsfsdffsfsdf-423432-ffsdfsfsd-fsdfsdfs-fddsfdsfdsfs	Duration: 5271.58 ms	Billed Duration: 7564 ms	Memory Size: 128 MB	Max Memory Used: 128 MB	Init Duration: 2292.01 ms

Request ID: csadfesf-324324-sdfdsffds-fsdfsd-xcvzxzvzvvzv

### Task 5.3: Review data in the consumption zone S3 bucket

Goto S3 and review your work.

Note: The company can use this data to identify the top few products that customers leave in shopping carts, and then offer promotional discounts for these products.

## Conclusion

I have successfully done the following:

- Used Amazon S3 as the storage layer of a data lake.
- Organized data into layers (or zones) in Amazon S3.
- Configured an S3 event notification to invoke an AWS Lambda function.
- Created an Amazon EventBridge rule to invoke the Lambda function.

References:

[What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
[What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
[[What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)]]






















