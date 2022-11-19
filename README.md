
# Word Counter using AWS Lambda

This project is just simple demonstration of how AWS Lambda works using Python as runtime. Everytime a user uploads a text file in a particular bucket which is the trigger, the function will run to count the words in that text file and invoke AWS SNS to send a message containing the number of words to email subscriber. 

![Architecture](https://raw.githubusercontent.com/emeteriocatabayjr/Word-Counter-using-AWS-Lambda/master/WordCounter.drawio.png)



## Tech Used

- Amazon S3
- Amazon SNS
- AWS Lambda
- Gmail Account



## Code

```
import json
import boto3

s3 = boto3.client('s3') # client that representing s3
sns = boto3.client('sns') # client that presenting sns

def lambda_handler(event, context):

    bucket = event['Records'][0]['s3']['bucket']['name'] # get the bucket name from the event
    key = event['Records'][0]['s3']['object']['key'] # get the key name from the event
    
    data = s3.get_object(Bucket = bucket, Key = key) # get the object
    contentOfFile = str(data['Body'].read()) # get the content inside the file
    splitContentToList = list(contentOfFile.split()) # use split() and list() to separate each word
    countTheList = str(len(splitContentToList)) # used len() to count each word
    
    sns_message = str("The word count in the file "+ key +" is "+ countTheList) # the body of the email 
    print(str(sns_message))

    subject= "Word Count Result" # subject of the email
    print(subject)

    sns_response = sns.publish(
    TargetArn='SNS ARN is here',
    Message= str(sns_message),
    Subject= str(subject)
    ) # this going to send to SNS topic

```


