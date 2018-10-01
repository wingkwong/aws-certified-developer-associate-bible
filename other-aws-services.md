# Other AWS Services

### SQS

- What is SQS?
  - Amazon SQS is a web service that gives you access to a message queue that can be used to store messages while waiting for a computer to process them.
  - Amazon SQS is a distributed queue system that enables web service applications to quickly and reliably queue messages that one component in the application generates to be consumed by another component. A queue is a temporary repository for messages that are awaiting processing
  - Using Amazon SQS, you can decouple the components of an application so they run independently, easing message management between components
  - Any component of a distributed application can store messages in the queue. Messages can contain up to 256 KB of text in any format. Any component can later retrieve the messages programmatically using the Amazon SQS API
  - The queue acts as a buffer between the component producing and saving data, and the component receiving the data for processing. This means the queue resolves issues that arise if the producer is producing work faster than the consumer can process it, or if the producer or consumer are only intermittently connected to the network

- Queue Types
  - Standard Queues (default)
  - FIFO Queues (First-In-First-Out)

- SQS Key Facts
  - SQS is pull-based, not pushed-based
  - Messages are 256 KB in size
  - Messages can be kept in the queue from 1 minute to 14 days
  - Default retention period is 4 days
  - SQS guarantees that your messages will be processed at least once

- SQS Visibility Timeout
  - The amount of time that the message is invisible in the SQS queue after a reader picks up that message
  - Provided the job is processed before the visibility time out expires, the message will then be deleted  from the queue
  - If the job is not processed within that time, the message will become visible again and another reader will process it. This could result in the same message being delivered twice
  - Default Visibility Timeout is 30 seconds
  - Increase it if your tasks takes > 30 seconds
  - Maximum is 12 hours

- SQS Long Polling
  - Amazon SQS long polling is a way to retrieve messages from your Amazon SQS queues
  - While the regular short polling returns immediately (even if the message queue being polled is empty), long polling doesn't return a response until a message arrives in the message queue, or the long poll times out
  - As such, long polling can save your money

- SQS Exam Tips
  - SQS is a distributed message queueing system
  - Allows you to decouple the components of an application so that they are independent
  - Pull-based, not push-based
  - Standard Queues (default) - best-effort ordering; message delivered at least once
  - FIFO Queues (First In First Out) - ordering strictly preserved, message delivered once, no duplicates. e.g good for banking transactions which need to happen in strict order.
  - Visibility Timeout
    - Default is 30 seconds - increase if your task takes > 30 seconds to complete
    - Max 12 hours
  - Short Polling - returned immediately even if no messages are in the queue
  - Long Polling - polls the queue periodically and only returns a response when a message is in the queue or the timeout is reached


### Simple Notification Service (SNS)

- SNS Exam Tips
  - SNS is a scalable and highly available notification service which allows you to send push notifications from the cloud
  - Variety of messages formats supported: SMS text message, email, Amazon SQS queues, any HTTP endpoint
  - Pub-sub(Publish-Subscribe) model whereby users subscribe to topics (access points)
  - It is a push mechanism, rather than a pull(poll) mechanism

### SNS V.S SES
- Exam Tips
  - SES:
    - Remember that SES is for email only
    - It can be used for incoming and outgoing mail
    - It is not subscription based, you only need to know the email address
  - SNS:
    - SNS supports multiple formats (SMS, SQS, HTTP, email)
    - Push Notifications only
    - Pub/sub model: consumers must subscribe to a topic
    - You can fan-out messages to large number of recipients (e.g. multiple clients each with their own SQS queue

### Elastic Beanstalk 101

- Exam Tips
  - Deploys and scales your web applications including the web application server platform where required
  - Supports widely sued programming technologies - Java, PHP, Python, Ruby, Go, Docker, .NET, Node.js
  - And application server platforms like Tomcat, Passenger, Puma, and IIS
  - Provisions the underlying resources for you
  - Can fully manage the EC2 instances for you or you can take full administrative control
  - Updates, monitoring, metics, and health checks all included

- Updating Elastic Beanstalk Exam Tips:
  - All at once
    - Service interruption while you update the entire environment at once
    - To roll back, perform a further all at All at Once upgrade
  - Rolling
    - Reduced capacity during deployment
    - To roll back, perform a further rolling update
  - Rolling with Additional Batch
    - Maintains full capacity
    - To roll back, perform a further rolling update
  - Immutable
    - Preferred option for mission critical production systems
    - Maintains full capacity
    - To roll back, just delete the new instances and autoscaling group

- Advanced Elastic Beanstalk Exam Tips
  - You can customize your Elastic Beanstalk environment by adding configuration files
  - The fils are written in YAML or JSON
  - Files have a .config extension
  - The .config files are saved to the .ebextensions folder
  - Your .ebextensions folder must be located in the top level directory of your application source code bundle
