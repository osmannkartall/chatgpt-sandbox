# Uploading Data to S3 with Lambda

Here are the steps to upload data to S3 with Lambda Functions:

1. Create an S3 bucket:

```
aws s3 mb s3://my-bucket
```

2. Create a new Lambda function:

```
aws lambda create-function --function-name my-function --runtime python3.8 --handler index.lambda_handler --role arn:aws:iam::123456789012:role/lambda-role --zip-file fileb://function.zip
```

3. Create a new IAM role with S3 permissions:

```
aws iam create-role --role-name lambda-role --assume-role-policy-document file://trust-policy.json

aws iam attach-role-policy --role-name lambda-role --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

4. Create a Python file named index.py with the following code:

```python
import json
import boto3

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket_name = 'my-bucket'
    file_name = 'file.txt'
    s3.upload_file(file_name, bucket_name, file_name)
    return {
        'statusCode': 200,
        'body': json.dumps('File uploaded successfully')
    }
```

5. Zip the index.py file:

```
zip function.zip index.py
```

6. Upload the function code to Lambda:

```
aws lambda update-function-code --function-name my-function --zip-file fileb://function.zip
```

7. Test the Lambda function:

```
aws lambda invoke --function-name my-function --payload '{}' output.txt
```

8. Check the S3 bucket to verify that the file was uploaded:

```
aws s3 ls s3://my-bucket
```

That's it! Your Lambda function should now upload the file to the S3 bucket when it is triggered.