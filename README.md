# AWS Practical Task: Deploy an Application with GitHub Actions and Amazon S3

**URL:** [http://luisalbertolj.s3-website-us-east-1.amazonaws.com/](http://luisalbertolj.s3-website-us-east-1.amazonaws.com/)

**Created by:**  
Luis Alberto Luisjuan Guerrero

## 1. Introduction
This document serves as the documentation for the project to deploy an application using GitHub Actions and Amazon S3, aimed at addressing the challenges posed by the current business environment in terms of cloud application and service management.

### 1.1 What should be done:
To complete the final task, you will need an AWS Free Tier account.

**Necessary knowledge:** 
- Basic skills in AWS
- Basic skills in GitHub
- Fundamental skills in API

**Task:** 
- Create Amazon S3 and configure the bucket for the purpose of hosting a static website with public access.
- Create a GitHub repo with a simple personal website.
- Deploy an application with GitHub Actions and Amazon S3.

### 1.2 Definitions of done:
- You have created, launched, and have a working simple personal website based on AWS.
  - Important: The DNS record must be in the AWS domain, can be in any format (including the default format), and provided by AWS.
- You have created a private repository on GitHub.
  - Important: You need to describe in detail and add screenshots of the general steps for completing this lesson to the README.md file in your GitHub repository branch. The file should look like a step-by-step instruction for this task.

## 2. Requirements
- An AWS Free Tier account
- A GitHub account
- Basic knowledge of GitHub Actions

## 3. Solution Architecture
- Create an S3 bucket on AWS
- Configure the bucket for static website hosting
- Set up a GitHub repository with a simple personal website
- Configure GitHub Actions to deploy the website to the S3 bucket

## 4. Implementation

### Step 1: Create AWS Access Keys
1. Log in to your AWS account.
2. Navigate to the IAM service.
3. **Create a new user:**
   - Go to the **Users** section and click on **Add user**.
   - Provide a user name and select **Programmatic access**.

4. **Configure the user group to have S3 full access:**
   - Click on **Next: Permissions**.
   - Choose **Attach existing policies directly**.
   - Search for **AmazonS3FullAccess** and select it.

5. **Generate access key for programmatic access for this user:**
   - After creating the user, you’ll see an option to download the `.csv` file with the **Access Key ID** and **Secret Access Key**. Keep these credentials safe.

### Step 2: Create an S3 Bucket and Configure Permissions

1. **Add the access key ID and access key secret to your AWS CLI configuration:**
   - Run the AWS CLI command:
     ```sh
     aws configure
     ```
   - Enter your **Access Key ID** and **Secret Access Key**.
   - Set your default region and output format (e.g., `us-east-1` and `json`).

2. **Test your configuration by listing your S3 buckets:**
   ```sh
   aws s3 ls
