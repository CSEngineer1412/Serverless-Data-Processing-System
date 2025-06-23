import json
import boto3
import os

s3_client = boto3.client('s3')
sns_client = boto3.client('sns')

SNS_TOPIC_ARN = os.environ['SNS_TOPIC_ARN']

def lambda_handler(event, context):
    bucket = event['Records'][0]['s3']['bucket']['name']
    file_name = event['Records'][0]['s3']['object']['key']

    print(f"Processing file: {file_name} from bucket: {bucket}")

    message = f"File '{file_name}' was uploaded to bucket '{bucket}' and processed."
    sns_client.publish(TopicArn=SNS_TOPIC_ARN, Message=message, Subject='File Processed')

    return {
        'statusCode': 200,
        'body': json.dumps('File processed and notification sent.')
    }
