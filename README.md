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
     ```

3. **Create a bucket with the CLI:**
    ```sh
    aws s3 mb s3://your-bucket-name --region your-region
    ```

4. **Change the Public access permissions via the AWS Management Console:**
   - Navigate to the S3 service.
   - Select your newly created bucket.
   - Go to Permissions and disable Block all public access.
   - Save changes.

5. **Change the bucket policy using Boto3 in a Python script (e.g., set_bucket_policy.py):**
    ```python
    import boto3
    import json

    s3 = boto3.client('s3')

    bucket_name = 'your-bucket-name'
    policy = {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": f"arn:aws:s3:::{bucket_name}/*"
            }
        ]
    }

    policy_string = json.dumps(policy)
    s3.put_bucket_policy(Bucket=bucket_name, Policy=policy_string)
    ```

6. **Run the script:**
    ```sh
    python set_bucket_policy.py
    ```

### Step 3: Create a Static Website GitHub Repository with Actions CI/CD
Our GitHub Action workflow will be responsible for deploying the content of our repository (basically our website) to AWS S3 whenever the repository receives a commit-push. This will ensure that our live website is always up-to-date.

1. On your repository that contains the website, navigate to the “Actions” tab.
2. On the left sidebar, click on “New Workflow”.
3. Click on “Set up a workflow yourself”.
4. Give a name to your workflow file. The default name is “main.yml”. You can change it or allow the default one.
5. Paste this content inside the workflow:

    ```yaml
    name: deploy static website to AWS-S3

    on:
      push:
    jobs:
      deploy:
        runs-on: ubuntu-latest

        steps:
          - name: Checkout repository
            uses: actions/checkout@v2

          - name: Set up AWS CLI
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: us-east-1
          - name: Deploy to AWS S3
            run: |
              aws s3 sync . s3://your-bucket-name --delete
    ```

### Step 4: Configuring GitHub Environment Secrets

Remember the Access key we created earlier? Its time is NOW!

1. In your GitHub repository that contains the website, go to the Settings tab.
2. Scroll down to “Secrets and Variables” in the left sidebar and click on it.
3. Choose “Action” option.
4. Click on “New repository secret”.
5. Give the name “AWS_ACCESS_KEY_ID” to your variable.
6. Repeat the first three steps of “Generate Access Keys” section to view the access key you created.
7. Copy its content and paste it as the value for AWS_ACCESS_KEY_ID.
8. Click on “Add secret” to save the secret variable.
9. Do the same to create the “AWS_SECRET_ACCESS_KEY”. Make sure you copied the real value of the key’s secret, not key ID.

### Step 5: Make a Change and Check the Deployment

1. Commit the change.
2. Check the deployment workflow:
   - In S3 AWS Web Console, get the URL and test it!
