# Large-Scale Data Integration for Chatbots using Amazon S3 and Amazon Q for Business

[Amazon Q Business](https://aws.amazon.com/q/business/) is a generative-AI powered assistant that you can configure to answer questions, provide summaries, generate content, and complete tasks based on your enterprise data. In this solution we provide CloudFormation templates to deploy a sample Q Business Application with [AWS IAM Identity Center](https://aws.amazon.com/iam/identity-center/) integration. 

## Prerequisites

For this solution to work, the following prerequisites are needed:
* A new or existing AWS account that will be the data collection account
* Corresponding ~[AWS Identity and Access Management](https://aws.amazon.com/iam/)~ (IAM) permissions to create S3 buckets and deploy CloudFormation stacks

## Solution Deployment:

This repository has three templates: IDCResources.yaml, QBusinessApp.yaml and QBusinessDataWorkflow.yaml.

### Configure the Data Ingestion:

This creates the following:
* S3 data bucket.
* Ingestion Lambda function.
* Processing Lambda function.
* Step Functions workflow.

⠀The data ingestion in this example fetches and processes public data from the Amazon Q Business and Amazon SageMaker official documentation in PDF format. Specifically, the Ingest Data Lambda downloads the raw PDF documents, temporarily stores them in S3 and passes their S3 urls to the Process Data Lambda, which performs the PII redaction (if enabled) and stores the processed documents and their metadata to the S3 path indexed by the Amazon Q Business app.
Adapt the Steps Functions Lambda code for ingestion and processing according to your own internal data, ensuring the documents and metadata are in a valid format for Amazon Q Business to index, and are properly redacted for any PII data.

### Configure IAM Identity Center:

Note: You can only have one Amazon IAM Identity Center instance per account. If your account already has an Identity Center instance, please skip this step and proceed to Configure the Amazon Q for Business Application:

Deploy the IDCResources CloudFormation template.

You will need to add details for a user such as username, email, name, surname.


After deploying the CloudFormation template you will receive an email where you will need to accept the invitation and change the password for the user. 

Before logging in you will need to deploy the Amazon Q for Business application.

### Configure the Amazon Q for Business Application:

Deploy the QBusinessApp CloudFormation template.

You will need to add details such as the identity center stack name deployed previously and the S3 bucket name provisioned by the data ingestion stack.

After deploying the CloudFormation template you will need to do the following:

* Navigate to the Amazon Q Business Console.
* Select Applications on the left pane.
* Select the application provisioned.
* Under Manage access and subscriptions select the Users tab.
* Then select the user you specified when the CloudFormation stack was deployed.
* Click Edit Subscription.
* Under New subscription select Business Pro.
* Select Confirm then Confirm again.

Now you can log in using the user you have specified. You can find the URL for the Web experience under Web experience settings.

If you are unable to log in ensure that the user has been verified. Before using the Amazon Q Business application, the data source needs to be synchronized.

### Sync Datasource:

Before you can use the Amazon Q Business application, the data source needs to be synchronized. The application’s data source is configured to sync hourly. It may take some time to synchronize. 
 
When the synchronization is complete, you should now be able to access the application and ask questions.


## Clean up

After you’re done testing the solution, you can delete the resources to avoid incurring additional charges. See the Amazon Q Business pricing page for more information. Make sure to delete the CloudFormation stack provisioned as follows:

1. Delete the Amazon Q for Business Application stack.
2. Delete the IAM Identity Center stack.
3. Delete the Data Ingestion stack.
4. For each deleted stack check for any resources that were skipped in the deletion process.
   1. Such as S3 buckets.
5. Delete skipped resources on the console.

## Disclaimer

The sample code provided in this solution is for educational purposes only and users should thoroughly test and validate the solution before deploying it in a production environment.