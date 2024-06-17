# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Automatically Start and Stop EC2 Instances with AWS Instance Scheduler.


## <a name="introduction">🤖 Introduction</a>

In this hands-on demo, I’ll give you a brief overview of how Instance Scheduler works, then we’ll deploy the solution using CloudFormation.  From there, we’ll configure periods and schedules in a DynamoDB table.  Finally, we’ll tag the instance that we want to shut down, and test that it gets shut down at the time we specify.

## <a name="design">📐 Project Architecture</a>

![aws](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/c2dcf937-80b0-4a00-9c15-1c9934a6e764)



## <a name="steps">☑️ Steps</a>

1. The Amazon CloudFormation template sets up an Amazon CloudWatch event at a customer-defined interval. This event invokes the Instance Scheduler Amazon Lambda function. During configuration, the user defines the Amazon Web Services Regions and accounts, as well as a custom tag that Amazon Web Services Instance Scheduler will use to associate schedules with applicable Amazon EC2 and Amazon RDS instances.

2. These values are stored in Amazon DynamoDB, and the Lambda function retrieves them each time it runs. You can then apply the custom tag to applicable instances.

3. During initial configuration of the Instance Scheduler, you define a tag key you will use to identify applicable Amazon EC2 and Amazon RDS instances. When you create a schedule, the name you specify is used as the tag value that identifies the schedule you want to apply to the tagged resource. For example, a user might use the solution’s default tag name (tag key) Schedule and create a schedule called uk-office-hours. To identify an instance that will use the uk-office-hours schedule, the user adds the Schedule tag key with a value of uk-office-hours.



## ➡️ Step 1 - Create CloudFormation

To get started, we have Template Sample you can use for this demo, click the button below, this is going to open your AWS Console and it will launch a CloudFormation Template with all the details filled in.

<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Finstance-scheduler-on-aws%2Flatest%2Finstance-scheduler-on-aws.template&redirectId=SolutionWeb" target="_blank">
<img src="https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/5e48af4c-7949-4ada-8ec4-948b8003e64a
" alt="Project Banner">
</a>



