u# AWS CLI Text-Based Game Deployment

This project demonstrates how to deploy a simple text-based game using AWS CLI and AWS Lambda.

## Prerequisites

* AWS CLI installed and configured
* Python 3.x installed
* An AWS account

## Step-by-Step Instructions

### 1. Configure AWS CLI

```
aws configure
```

Enter your AWS credentials, region, and output format.

### 2. Create IAM Role for Lambda

Create a file named `trust-policy.json` with the following content:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Run:

```
aws iam create-role \
  --role-name lambda-execute-role \
  --assume-role-policy-document file://trust-policy.json
```

### 3. Attach Policy to Role

```
aws iam attach-role-policy \
  --role-name lambda-execute-role \
  --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

### 4. Create Python Game

Create a file named `game.py`:

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to the guessing game! Guess a number between 1 and 5.'
    }
```

Zip the file:

```
zip game.zip game.py
```

### 5. Create Lambda Function

Replace `<YOUR_ACCOUNT_ID>` with your AWS Account ID:

```
aws lambda create-function \
  --function-name TextGameFunction \
  --runtime python3.9 \
  --role arn:aws:iam::<YOUR_ACCOUNT_ID>:role/lambda-execute-role \
  --handler game.lambda_handler \
  --zip-file fileb://game.zip
```

### 6. Invoke Lambda Function

```
aws lambda invoke \
  --function-name TextGameFunction \
  response.json
```

### 7. View the Response

```
cat response.json
---

### 7. View the Response

```'''
