# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Automatically Start and Stop EC2 Instances with AWS Instance Scheduler.


## <a name="introduction">🤖 Introduction</a>

In this hands-on demo, I’ll give you a brief overview of how Instance Scheduler works, then we’ll deploy the solution using CloudFormation.  From there, we’ll configure periods and schedules in a DynamoDB table.  Finally, we’ll tag the instance that we want to shut down, and test that it gets shut down at the time we specify.

## <a name="design">📐 Project Architecture</a>

![aws](https://github.com/julien-muke/aws-instance-scheduler/assets/110755734/c2dcf937-80b0-4a00-9c15-1c9934a6e764)



## <a name="steps">☑️ Steps</a>

* Create a Create NextJS App
* Push Source Code to GitHub
* Create S3 Bucket
* Setting permissions for website access
* Upload the app to S3 Bucket
* Create CloudFront Distribution
* Setup a CI/CD Pipeline with GitHub Actions and AWS
* Set up IAM roles to connect GitHub Actions to actions in AWS
* Configuring OpenID Connect in Amazon Web Services


## ➡️ Step 1 - Create and configure a Next.js 13 app