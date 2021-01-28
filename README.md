# s3-activity-ledger


## Workshop Description

In this workshop, you will learn how to record specified S3 API calls that happen in your account into QLDB.
Why record S3 API calls

#### Why S3 

S3 is a powerful and useful cloud object storage solution. S3 can be used for your most sensitive documents, as well as data that you would like to distribute the public or more simply put, the open internet.

By recording API calls to S3, we can detect important changes to the buckets. In this workshop, we will check for creation and deletion of buckets. We will also check for changes to the S3 buckets Access Control List and service level policy.

#### Why QLDB

Amazon Quantum Ledger Database (Amazon QLDB) is a fully managed ledger database owned by a central trusted authority that provides a transparent, immutable, and cryptographically verifiable transaction log of all of your application changes. Amazon QLDB tracks each and every application data change and maintains a complete and verifiable history of changes over time.

By using QLDB, your System Administrators and Developers will know that if they make important bucket level changes to S3. That there will be a record of the changes documented to QLDB - forever. They can change the S3 bucket back to the way it was, but they cannot change what has been entered into QLDB.

## Prerequisites

#### AWS Account

In order to complete these workshops you'll need a valid active AWS Account with Admin permissions. The code and instructions in these workshops assume only one student is using a given AWS account at a time. If you try sharing an account with another student, you'll run into naming conflicts for certain resources.

Use a personal account or create a new AWS account to ensure you have the necessary access. This should not be an AWS account from the company you work for.

#### Programming

These workshops assume that you are using a Cloud IDE environment. We recommend you use the latest version of Chrome or Firefox to complete this workshop.

Basic python knowledge is sufficient to consume these workshops.

## Lets get started.

### Step 1

#### a: Create QLDB Ledger


If this is your first time creating in QLDB. You should be directed to the QLDB home page like below.

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/create-qldb-step1.png)

#### b: Create QLDB Ledger

Click on the Create Ledger button and you will be prompted to enter a Ledger name. Enter a name of your choosing and click Create ledger to move onto the next step.


![alt text](https://account-activity.awsqldbworkshops.com/overview/images/create-qldb-step2.png)

 > Ledger creation usually takes less than 1 minute.
 
 #### c: Create a Ledger Table

Once the Ledger is created, navigate to the Query editor on the left of the screen. As shown below.

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/create-qldb-step3a.png#center40)

Once in the Query editor, enter the below QLDB SQL Query in the query editor.

```
CREATE TABLE s3activity;
```

Now click Run to create the table. Under Status, you should see Success shortly after running. As shown below.

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/create-qldb-step3b.png)

### Step 2

#### a: Execute CloudFormation template

The cloudformation template will setup the required lambda functions and permissions for this lab. The template will create one java lambda function to handle the QLDB logic. A python lambda function will trigger and pass required inputs to the java function. Least privilege IAM roles will be created for both lambda functions.

Deploy the template

TODO

#### b: Select region

Ensure that you are in the desired region and click Next

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/cloudformation-step2.png)

#### c: Parameters

Accept the defaults or provide custom inputs for the Parameters section. Click Next when ready.

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/cloudformation-step3.png)

#### d: Confirm and Create stack

Scroll to the bottom of the page and accept the Capabilities box as shown below. Click Create stack when ready.

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/cloudformation-step5.png)

### Step 3: EventBridge Rule

#### a: Go to EventBridge

From the services tab in the console, enter "EventBridge"

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/eventbridge-rule-step1.png#center60)

If this is your first time in EventBridge, the homepage will appear like below.

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/eventbridge-rule-step1b.png#center80)

#### b: Create Rule

Click on Create rule and you will be moved to the configuration page. Enter a name and description for the rule.

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/eventbridge-rule-step2.png#center80)

#### c: Define Pattern

In the Define pattern section, select Event pattern.

Under Event matching pattern, select Pre-defined pattern by service.

For the Service provider, select AWS.

For the Service name, select Simple Storage Service (S3).

#### d: Define Event Type

For the Event type, select Bucket Level Operations.

Then, select Specific operation(s) and add the following operations.

    CreateBucket
    DeleteBucket
    PutBucketPolicy
    DeleteBucketPolicy
    PutBucketAcl
    DeleteBucketPublicAccessBlock
    
> Other operations have importance but we for this workshop, we will focus on the above 6 operations 
 
Your configuration's should match as shown below.
 
![alt text](https://account-activity.awsqldbworkshops.com/overview/images/eventbridge-rule-step4.png#center60)
 
#### e: Configure event bus

Select AWS default event bus for the rule.

Enable Enable the rule on the selected event bus.

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/eventbridge-rule-step5.png#center60)

#### f: Add Target

Next, for the Target, select Lambda Function.

For the Function, enter the python function that was previously created with the cloudformation template.

> The configuration for the version/alias and input can remain as the defaults

![alt text](https://account-activity.awsqldbworkshops.com/overview/images/eventbridge-rule-step6.png)

#### g: Deploy Rule

There is the option to add tags if desired.

Click, "Create rule"

### Step 4: Check s3 activity from QLDB


