# AWS Lambda Hello World Deployment via Jenkins

## Overview
This project creates a simple AWS Lambda function that prints "Hello, World!" and runs every 15 minutes. The deployment is automated using a Jenkins pipeline.

## Components
- `lambda_function.py`: Contains the Lambda handler.
- `Jenkinsfile`: CI/CD pipeline that:
  - Zips the code
  - Creates or updates the Lambda function
  - Sets a CloudWatch trigger to run every 15 minutes

## Prerequisites
- AWS CLI configured with necessary permissions
- Jenkins with AWS CLI available on agents
- IAM role with Lambda execution and CloudWatch Events permissions

## Deployment Steps
1. Commit the code to your Git repository.
2. Setup Jenkins job with this repo.
3. Run the pipeline.

## Notes
- Ensure the IAM role passed to Lambda has basic execution permissions.
- The CloudWatch Event rule is named `hello-schedule`.

## Example Output
