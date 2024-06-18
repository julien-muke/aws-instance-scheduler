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

Step 2. Configure periods and schedules
<br>* Create a period and set the applicable fields for the period
<br>* Create a schedule and set the applicable fields for the schedule

Step 3. Creating an EC2 instance and Tag your instances
<br>* Apply the custom tag to applicable resources

Step 4. Test Instance Scheduler


## ➡️ Step 1 - Create CloudFormation

To get started, sign in to the AWS Management Console and click the button below to launch the aws-instance-scheduler Amazon CloudFormation template.

<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Finstance-scheduler-on-aws%2Flatest%2Finstance-scheduler-on-aws.template&redirectId=SolutionWeb">
<img src="https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/5e48af4c-7949-4ada-8ec4-948b8003e64a" target="_blank">
</a>

1. The template is launched in the US East (N. Virginia) Region by default. To launch the Instance Scheduler in a different Region, use the region selector in the console navigation bar.
2. On the Select Template page, verify that you selected the correct template and choose Next.


![CloudFormation-us-east-1(1)](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/d6b96bf4-faec-4880-8cba-47f64b18f569)

3. On the Specify Details page, assign a name to your solution stack `instance-scheduler`
4. Under Parameters, review the parameters for the template, and modify them as necessary. This solution uses the following default values, choose Next.

![Screenshot 2024-06-17 at 13 29 48](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/13f19876-057e-424c-bf68-29a076f41e08)

5. Leave everything as default and click "Next"
6. For "Configure stack options" leave everything as default and click "Next"
7. On the Review page, review and confirm the settings. Check the box acknowledging that the template will create Amazon Identity and Access Management (IAM) resources.
8. Choose Create to deploy the stack.

![Screenshot 2024-06-17 at 13 31 58](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/901f5301-1bed-415d-9038-95e2e3e51306)

You can view the status of the stack in the Amazon CloudFormation console in the Status column. You should see a status of CREATE_COMPLETE in roughly five (5) minutes

This is going to create a DynamoDB Table, some IAM Roles, a Lambda Function, it will set up KMS and SNS and CloudWatch.

As you can see below our CloudFormation is completed.

![Screenshot 2024-06-17 at 14 30 09](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/96093e34-a656-4d91-81f5-f2d863eb1005)


## ➡️ Step 2 - Configure periods and schedules

When you deploy the Amazon CloudFormation template, the solution creates an Amazon DynamoDB table that contains sample period rules and schedules that you can use as a reference to create your own custom period rules and schedules.

To create a period rule, you can use the Amazon DynamoDB console:

1. Navigate to Amazon DynamoDB console

Remember CloudFormation created some tables for us, if you click on tables you should have one called `Instance-scheduler-ConfigTable-XXXXXXXXXX` click on the table.

![Screenshot 2024-06-17 at 15 45 28](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/3574d87e-f820-4a6d-a1b1-3d0bfb53af03)

2. Choose "Explore table items" to actually see the data in the table.

![Screenshot 2024-06-17 at 15 46 06](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/3a98f0a4-0308-4b7d-8456-9bc6cbc7547f)


3. Scrolling down, you will see Schedules and Periods that we can go configure. The recommendation is to take one that exists and just update it for what you need. 

Let's find a period that works for us for example: `office-hours` has a begintime of `9:00` and endtime of `17:00` and applies Monday to Friday. Choose "Period" to modify it.

![Screenshot 2024-06-17 at 15 48 08](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/c939cecc-f733-4f3c-a4af-abacd41e0d1a)

4. For this demo, i want to show you that the instance gets stopped at a particular time, i'm going to change this to just a few minutes away from where we are now, which will be `10:05` and then Save changes.

![Screenshot 2024-06-17 at 16 01 46](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/c391dc97-14f2-4e35-b5d1-e7bf3a3ef64d)


5. For the Schedule: 
* Let's choose the schedule that we want, for example: `seattle-office-hours` hours that might actually work for me I'm on Seattle time (for this time to work make you sure you are in `us-east-1` regoin). 
* If we scroll over we'll see the description and then we'll see that it's already configured to use the `office hours` period which is the one that i just updated, it's set to U.S Pacific time. You can change any of these to work for the time that you're in


![Screenshot 2024-06-17 at 15 48 08](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/fb735606-adf8-441b-bff2-aa19c86c8310)


## ➡️ Step 3 - Creating an EC2 instance and Tag your instances

Now we need an actual instance to start and stop using this scheduler.

Let's create a new EC2 Instance, but if you already have one that's fine we just need to add a tag to.

NOTE: The scheduler also works on RDS Instances.

To launch an instance:

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
2. From the EC2 console dashboard, in the Launch instance pane, choose Launch instance.

![Screenshot 2024-06-18 at 12 30 41](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/979e5f55-fac1-426f-8c58-4bb3d30bae9a)

3. Under Name and tags, for Name, enter a descriptive name for your instance 

![Launch-an-instance-EC2-us-east-1(1)](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/b73689af-de1e-47ee-b76d-d659391b39a2)

4. Under Application and OS Images (Amazon Machine Image), we recommend that you choose Amazon Linux.
5. From Amazon Machine Image (AMI), select an AMI that is marked Free Tier eligible.
6. Under Key pair (login), choose Proceed without a key pair.
7. Under Network settings, Create secrty group, choose "Allow SSH traffic from" and "Allow HTTP traffic from the internet"
8. Keep the rest as default and choose "Launch instance"


## ➡️ Step 4 - Creating an EC2 instance and Tag your instances

To star or stop EC2 instances from the dashboard, you can add the pre-determined tag at any point. These changes update on the dashboard in approximately five minutes.

To add an Amazon EC2 tag: 

1. Select the instance you want to onboard to the dashboard `Test Schedule`
2. Select the Tags tab. Choose Manage tags. 

![Screenshot 2024-06-17 at 16 03 12](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/5e24bb38-d47d-4ad0-96c5-85c0aef60174)

3. Choose Add tag to the instance and provide the key-value pair you provided during deployment, in my case:

* Key = `Name`      Value = `Test Schedule` 
* Key = `Schedule`  Value = `seattle-office-hours`


![Screenshot 2024-06-17 at 16 03 52](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/5c6e7682-3d09-442e-afaf-54e34a6894ab)


## ➡️ Step 5 - Testing that the scheduler automatically stops the instance

Now I've got a few minutes before the 10:05 endtime starts, let's make sure this gets up and running and then we'll see that it's automatically shut down at 10:05.

If we look at the clock this might vary a little bit from the time on AWS but it's currently 10:05

![Screenshot 2024-06-17 at 16 06 07](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/d8ac779d-7d77-434f-848e-5d0a366e349d)

If I do a refresh, as you can see below the instance is stopping at exact 10:05

![Screenshot 2024-06-17 at 16 06 20](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/442a5381-4a96-4ca9-867d-637660f81b9b)


All of that's happening automatically behind the scenes based on the Schedule and the Period that we set up and the Tag.
The Lambda function is running stopping the instance and if i were to wait to the begintime in the Period of 9:00 am it would automatically get started again.

This can be a great way to save you some money on evenings and weekends or just any scheduled time when you know you don't need your machine up and running.