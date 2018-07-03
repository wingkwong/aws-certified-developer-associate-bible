# EC2 & Getting Setup

### EC 101 - Part 1
- What is EC2 (Elastic Compute Cloud)?
  - Amazon EC2 changes the economics of computing by allowing you to pay only for capacity that you actually use. Amazon EC2 provides developers the tools to build failure resilient applications and isolate themselves from common failure scenarios

- EC2 Options
  - On Demand - allows you to pay a fixed rate by the hour (or by the second) with no commitment
  - Reserved - provides you with a capacity reservation, and offer a significant discount on the hourly charge for an instance. 1 Year or 3 Year Terms
  - Spot - enables you to bid whatever price you want for instance capacity, providing for even greater savings if your applications have flexible start and end times
  - Dedicated Hosts - physical EC2 server dedicated for your use. Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound software licenses.

- On Demand
  - Perfect for users that want the low cost and flexibility of Amazon EC2 without any up-front payment or long-term commitment
  - Applications with short term, spiky, or unpredictable workloads that cannot be interrupted
  - Applications being developed or tested on Amazon EC2 for the first time

- Reserved Instances
  - Application with steady state or predictable usage
  - Applications that require reserved capacity
  - Users can make up-front payments to reduce their total computing costs even further
    - Standard RIs (Up to 75% off on-demand)
    - Convertible RIs (Up to 54% off on-demand) feature the capability to change the attributes of the RI as long as the exchange results in the creation of Reserved Instances of equal or greater value
    - Schedule RIs are available to launch within the time window you reserve. This option allows you to match your capacity  reservation to a predictable recurring schedule that only requires a fraction of a day, a week or a month

  - Spot Instances
    - Applications that have flexible start and end times
    - Applications that are only feasible at very low compute prices
    - Users with an urgent need for large amounts of additional computing capacity

  - Dedicated Hosts
    - Useful for regulatory requirements that may not support multi-tenant virtualization
    - Great for licensing which does not support multi-tenancy or cloud deployments
    - Can be purchased On-Demand (hourly)
    - Can be purchased as a Reservation for up to 70% off the On-Demand price

  - EC2 Instance Type
  ![EC2 Instance Type](http://scriptcrunch.com/wp-content/uploads/2016/08/instance-deatils_mini.jpg)

  - EBS (Elastic Block Store)
    - Amazon EBS allows you to create storage volumes and attach them to Amazon EC2 instances. Once attached, you can create a file system on top of these volumes, run a database, or use them in any other way you would use a block device. Amazon EBS volumes are placed in a specific Availability Zone, where they are automatically replicated to protect you from the failure of a single component

  - Exam Tips
    - If a spot instance is terminated by Amazon EC2, you will not be charged for a partial hour of usage. However, if you terminate the instance yourself, you will be charged for the complete hour in which the instance ran
    - SSD
      - General Purpose SSD - balances price and performance for a wide variety of workloads
      - Provisioned IOPS SSD - highest-performance SSD volume for mission-critical low-latency or high-throughput workloads
    - Magnetic
      - Throughput Optimized HDD - low cost HDD volume designed for frequently accessed, throughput-intensive workloads
      - Cold HDD - lowest cost HDD volume designed for less frequently accessed workloads
      - Magnetic - previous generation. Can be a boot volume.

### EC 101 - Part 2

- EBS consists of
  - SSD, General Purposes - GP2 - (Up to 10,000 IOPS)
  - SSD, Provisioned IOPS - I01 - (More than 10,000 IOPS)
  - HDD, Throughput Optimized - ST1 -frequently accessed workloads
  - HDD, Cold - SC1 - less frequently accessed data
  - HDD, Magnetic - Standard - cheap, infrequently accessed storage
- You cannot mount 1 EBS volume to multiple EC2 instances, instead use EFS
- EC2 Instance Types
  - D - Density
  - R - RAM
  - M - Main choice for general purpose apps
  - C - Compute
  - G - Graphics
  - I - IOPS
  - F - FPGA
  - T - Cheap general purpose (think T2 Micro)
  - P - Graphics (think Pics)
  - X - Extreme Memory


### Launch Our First EC2 Instance

- Summary
  - Termination Protection is turned off by default, you must turn it on
  - On an EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated
  - EBS Root Volumes of your DEFAULT AMI's cannot be encrypted. You can also use a third party tool (such as bit locker etc) to encrypt the root volume, or this can be done when creating AMI in the AWS console or using the API
  - Additional Volumes can be encrypted
