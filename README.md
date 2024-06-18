# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Automatically Start and Stop EC2 Instances with AWS Instance Scheduler.


## <a name="introduction">🤖 Introduction</a>

In this hands-on demo, I’ll give you a brief overview of how Instance Scheduler works, then we’ll deploy the solution using CloudFormation.  From there, we’ll configure periods and schedules in a DynamoDB table.  Finally, we’ll tag the instance that we want to shut down, and test that it gets shut down at the time we specify.

## <a name="design">📐 Project Architecture</a>

![aws](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/c2dcf937-80b0-4a00-9c15-1c9934a6e764)

## 📋 Overview

1️⃣ The Amazon CloudFormation template sets up an Amazon CloudWatch event at a customer-defined interval. This event invokes the Instance Scheduler Amazon Lambda function. During configuration, the user defines the Amazon Web Services Regions and accounts, as well as a custom tag that Amazon Web Services Instance Scheduler will use to associate schedules with applicable Amazon EC2 and Amazon RDS instances.

2️⃣ These values are stored in Amazon DynamoDB, and the Lambda function retrieves them each time it runs. You can then apply the custom tag to applicable instances.

3️⃣ During initial configuration of the Instance Scheduler, you define a tag key you will use to identify applicable Amazon EC2 and Amazon RDS instances. When you create a schedule, the name you specify is used as the tag value that identifies the schedule you want to apply to the tagged resource. For example, a user might use the solution’s default tag name (tag key) Schedule and create a schedule called uk-office-hours. To identify an instance that will use the uk-office-hours schedule, the user adds the Schedule tag key with a value of uk-office-hours.


## <a name="steps">☑️ Steps</a>

The procedure for deploying this architecture on AWS consists of the following steps:

Step 1. Launch the instance scheduler stack
<br>* Launch the Amazon CloudFormation template into your AWS account
<br>* Enter values for the required parameter: Stack Name
<br>* Review the other template parameters, and adjust if necessary

Step 2. Configure periods
<br>* Create a period and set the applicable fields for the period

Step 3. Configure schedules
<br>* Create a schedule and set the applicable fields for the schedule

Step 4. Tag your instances
<br>* Apply the custom tag to applicable resources

Step 5. Test Instance Scheduler


## ➡️ Step 1 - Create CloudFormation

To get started, we have Template Sample you can use for this demo, click the button below, this is going to open your AWS Console and it will launch a CloudFormation Template with all the details filled in.

<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Finstance-scheduler-on-aws%2Flatest%2Finstance-scheduler-on-aws.template&redirectId=SolutionWeb">
<img src="https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/5e48af4c-7949-4ada-8ec4-948b8003e64a" target="_blank">
</a>

For this temaplate we will choose:

1. "Choose an existing template" as the Prepare template
2. "Amazon S3 URL" as the Template source
3. Choose "Next"

![CloudFormation-us-east-1(1)](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/d6b96bf4-faec-4880-8cba-47f64b18f569)

4. Enter stack name `instance-scheduler`

![Screenshot 2024-06-17 at 13 29 48](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/13f19876-057e-424c-bf68-29a076f41e08)

5. Leave everything as default and click "Next"
6. For "Configure stack options" leave everything as default and click "Next"
7. Review the instance schedule
8. Click i acknowledge that AWS CloudFormation might create AM resources with custom names.

![Screenshot 2024-06-17 at 13 31 58](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/901f5301-1bed-415d-9038-95e2e3e51306)


This is going to create a DynamoDB Table, some IAM Roles, a Lambda Function, it will set up KMS and SNS and CloudWatch.
This might take several minutes to run you can always see how things are going by clicking on refresh.
As you can see below our CloudFormation is completed.

![Screenshot 2024-06-17 at 14 30 09](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/96093e34-a656-4d91-81f5-f2d863eb1005)




