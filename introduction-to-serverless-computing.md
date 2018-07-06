# Introduction to Serverless Computing

### Lambda

- What is Lambda?
  - Data Centres
  - Hardware
  - Assembly Code/Protocols
  - High Level Languages
  - Operating Systems
  - Application Layer/AWS APIs
  - AWS Lambda
    - A compute service where you can upload your code and create a Lambda function. AWS Lambda takes care of provisioning and managing the servers that you use to run the code. You don't have to worry about operating systems, patching, scaling, etc. You can use Lambda in the following ways
    - As an event-driven compute service where AWS Lambda runs your code in response to events. These events could be changes to data in an Amazon S3 bucket or an Amazon DynamoDB table
    - As a compute service to run your code in response to HTTP requests using Amazon API Gateway or API calls made using AWS SDKs

- What Languages?
  - Node.js
  - Java
  - Python
  - C#
  - Go

 - How is Lambda Priced?
  - Number of requests
    - First 1 million requests are free. $0.20 per 1 million request thereafter.
  - Duration
    - Duration is calculated from the time your code begins executing until it returns or otherwise terminates, rounded up to the nearest 100ms. The price depends on the amount of memory you allocate to your function. You are charged $0.00001667 for every GB-second used.

- Why is Lambda Cool?
  - NO SERVERS
  - Continuous Scaling
  - Super super super cheap

- Exam Tips
  - Lambda scales out (not up) automatically
  - Lambda functions are independent, 1 event = 1 function
  - Lambda is Serverless
  - Know what services are serverless
  - Lambda functions can trigger other lambda functions, 1 event can = x functions if functions trigger other functions
  - Architectures can get extremely complicated, AWS X-ray allows you to debug what is happening
  - Lambda can do things globally, you can use it to back up S3 buckets to other S3 buckets etc
  - Know your trigger
