AUTOMATING INFRASTRUCTURE WITH IAC USING TERRAFORM PART 1
INTRODUCTION
This project demonstrates how the AWS infrastructure for 2 websites that was built manually in project 15 is automated with the use of Terraform.

![alt text](./tooling_project_16.png.png)

The following outlines the steps taken:

STEP 0: Setting Up AWS CLI And S3 Buckets

![alt text](./s3.png)

![alt text](./iamuser.png)

So first i need to configure aws on my pc

First go to https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

download the aws cli msi software and install on my pc

open powershell and run the following commands to add my aws access id and secret key from my iam user to my pc

![alt text](./configaws.png)

next check the s3 bucket from powershell

![alt text](./awsls.png)

