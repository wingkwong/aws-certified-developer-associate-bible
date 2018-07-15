# Introduction To DynamoDB

- What is DynamoDB
  - Amazon DynamoDB is a fast and flexible NoSQL database service for all applications that need consistent, single-digit millisecond latency at any scale. It is a fully managed database and supports both document and key-value data models. Its flexible data model and reliable performance make it a great fit for mobile, web, gaming, ad-tech, IoT, and many other applications.

- DynamoDB
  - Stored on SSD storage
  - Spread Across 3 geographically distinct data centres
  - Choice of 2 consistency models
    - Eventual Consistent Reads (Default)
      - Consistency across all copies of data is usually reached within a second. Repeating a read after a short time should return the updated data. (Best Read Performance)
    - Strongly Consistent Reads
      - A strongly consistent read returns a result that reflects all writes that received a successful response prior to the read
  - Tables
  - Items (Think a row of data in a table)
  - Attributes (Think of a column of data in a table)
  - Supports key-value and document data structures
  - Key = The name of the data, Value = the data itself
  - Documents can be written in JSON, HTML or XML

- DynamoDB - Primary Keys
   - DynamoDB stores and retrieves data based on a Primary Key
   - 2 Types of Primary Keys
   - Partition Key - unique attribute e.g userID
    - Value of the Partition key is input to an internal hash function which determines the partition or physical location on which the data is stored
    - If you are using the Partition Key as your Primary Key, then no two items can have the same Partition Key
  - Composite Key (Partition Key + Sort Key) in combination
    - e.g Same user posting multiple times to a forum
      - Primary Key would a Composite Key consisting of
      - Partition Key - User ID
      - Sort key - Timestamp of the post
      - 2 items may have the same Partition Key, but they must have a different Sort Key
      - All items with the same Partition Key are stored together, then sorted according to the Sort Key Value
      - Allows you to store multiple items with the same Partition Key

- DynamoDB Access Control
  - Authentication and Access Control to DynamoDB is managed via AWS IAM
  - You can create an IAM user within your AWS account which has specific permissions to access and create DynamoDB Tables
  - You can create an IAM role with enables you to obtain temporary access keys that can be used to access AWS services like DynamoDB
  - You can also use a special IAM Condition to restrict user access to only their own records   

- DynamoDB Exam Tips
  - Amazon DynamoDB is a low latency NoSQL database
  - Consists of Tables Items and Attributes
  - Supports both document and key-value data models
  - Supported document formats are JSON, HTML, XML
  - 2 types of Primary Keys
    - Partition Key
    - Combination of Partition Key + Sort Key (Composite Key)

### Indexes Deepdive
- What is an Index?
  - In SQL databases, an index is a data structure which allows to you perform fast queries on specific columns in a table. You select the columns that you want included in the index and run your searches on the index - rather than on the entire dataset
  - In DynamoDB, 2 types of index are supported to help speed-up your DynamoDB queries:
    - Local Secondary Index
    - Global Secondary Index

- Local Secondary Index
  - Can only be created when you are creating your table
  - You cannot add, remove, or modify it later
  - It has the same Partition Key as your original table
  - But a different Sort Key
  - Give you a different view of your data, organized according to an alternative Sort Key
  - Any queries based on this Sort Key are much faster using the index than the main table
  - e.g. Partition Key: User ID; Sort Key: account creation date

- Global Secondary Index
  - You can create when you create your tables, or add it later
  - Different Partition Key as well as a Different Sort Key
  - So gives a completely different view of the data
  - Speeds up any queries relating to this alternative Partition and Sort Key
  - e.g. Partition Key: email address; Sort Key: last log-in date

 - Exam Tips:
  - Indexes enable fast queries on specific data columns
  - Give you a different view of your data, based on alternative Partition / Sort keys
  - Important to understand the differences

  | Local Secondary Index     | Global Secondary Index     |
  | :------------- | :------------- |
  | Must be created at when you create your table      | Can create any time - at table creation or after      |
  | Same Partition Key as your table      | Different Partition Key       |
  | Different Sort Key       | Different Sort Key      |
