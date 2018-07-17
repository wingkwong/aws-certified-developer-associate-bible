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

 - Indexes Deepdive Exam Tips:
  - Indexes enable fast queries on specific data columns
  - Give you a different view of your data, based on alternative Partition / Sort keys
  - Important to understand the differences

  | Local Secondary Index     | Global Secondary Index     |
  | :------------- | :------------- |
  | Must be created at when you create your table      | Can create any time - at table creation or after      |
  | Same Partition Key as your table      | Different Partition Key       |
  | Different Sort Key       | Different Sort Key      |

### Scan Vs Query

- What is a Query?
  - A Query operation finds items in a table based on the Primary Key attribute and a distinct value to search for
  - e.g. Select an item where the userID is equal to 212, will select all the attributes for that item, e.g. first name, surname, email etc
  - Use an optional Sort Key name and value to refine the results
  - e.g. if your Sort Key is a timestamp, you can refine the query to only select items with a timestamp of the last 7 days
  - By default, a Query returns all the attributes for the items but you can use the ProjectionExpression parameter if you want the query to only return the specific attributes you want
  - e.g. if you only want to see the email address rather than all the attributes
  - Results are always sorted by the Sort Key
  - Numeric order - by default in ascending order (1,2,3,4)
  - ASCII character code values
  - You can reverse the order by setting the ScanIndexForward parameter to false
  - By default, Queries are Eventually Consistent
  - You need to explicitly set the query to be Strong Consistent

- What is a Scan?
  - A Scan operation examines every item in the table
  - By default returns all data Attributes
  - Use the ProjectExpression parameter to refine the scan to only return the attributes you want

- Query or Scan?
  - Query is more efficient than a Scan
  - Scan dumps the entire table, then filters out the values to provide the desired result - removing the unwanted data
  - This adds an extra step of removing the data you don't want
  - As the table grows, the scan operation takes longer
  - Scan operation on a large table can use up the provisioned throughput fora  large table in just a single operation

- How to improve Performance
  - You can reduce the impact of a query or scan by setting a smaller page size which uses fewer read operations
  - e.g. set the page size to return 40 items
  - Larger number of smaller operations will allow other requests to succeed without throttling
  - Avoid using scan operations if you can: design tables in a way that you can use the Query, Get, or BatchGetItem APIs
  - By default, a scan operation processes data sequentially in returning 1MB increments before moving on to retrieve the next 1MB of data. It can only scan one partition at a time
  - You can configure DynamoDB to use Parallel scans instead by logically dividing a table or index into segments and scanning each segment in Parallel
  - Best to avoid parallel scans if your table or index is already incurring heavy read / write activity from other application

- Scan Vs Query - Exam Tips
  - A query operation finds items in a table using only the Primary Key attribute
  - You provide the Primary Key name and a distinct value to search for
  - A Scan operation examines every item in the table
  - By default returns all data attributes
  - Query results are always sorted by the Sort Key is there is one
  - Sorted in ascending order
  - Set ScanIndexForward parameter to false to reverse the order - queries only
  - Query operation is generally more efficient than a Scan
  - Reduce the impact of a query or scan by setting a smaller page size which uses fewer read operations
  - Isolate scan operations to specific tables and segregate them from your mission-critical traffic
  - Try Parallel scans, rather than the default sequential scan
  - Avoid using scan operations if you can: design tables in a way that you can use the Query, Get, or BatchGetItem APIs


### DynamoDB Provisioned Throughput

- DynamoDB Read and Write Capacity Units
  - DynamoDB Provisioned Throughput is measured in Capacity Units
  - When you create your table, you specify your requirements in terms of Read Capacity Units and Write Capacity Units
  - 1 x Write Capacity Unit = 1 x 1 KB Write per second
  - 1 x Read Capacity Unit = 1 x Strongly Consistent Read of 4KB per second OR
  - 2 x Eventually Consistent Reads of 4KB per second (Default)

  - Table with 5 x Read Capacity Units and 5 x Write Capacity
  - This configuration will be able to perform:
    - 5 x 4KB Strongly Consistent reads = 20 KB per second
    - Twice as many Eventually Consistent = 40KB
    - 5 x 1KB Writes per second
  - if your application reads or writes larger items it will consume more Capacity Units and will cost you more as well

- Strongly Consistent Reads Calculation
    - Your application needs to read 80 items (table rows) per second
    - Each item 3KB in size
    - You need Strongly Consistent Reads

    - First, calculate how many Read Capacity Units needed for each read:
      - Size of each item / 4KB
      - 3KB / 4KB = 0.75
    - Rounded-up to the nearest whole number, each read will need 1 x Read Capacity Unit per read operation
    - Multiplied by the number of reads per second = 80 Read Capacity Units required

- Eventually Consistent Reads Calculation
  - Do the same calculation and you get 2 x 4KB reads per second or double the throughput of Strongly Consistent reads
  - Size of each item / 4KB
  - 3KB / 4KB = 0.75
  - Rounded-up to the nearest whole number, each read will need 1 x Read Capacity Unit per read operation
  - Divided 80 by 2, so you only need 40 Read Capacity units for eventually consistent reads

- Write Capacity Units Calculation
  - You want to write 100 items per second
  - Each item 512 bytes in size

  - First, calculate how many Write Capacity Units needed for each read:
    - Size of each item / 1KB (for Write Capacity Units)
    - 512B / 1KB = 0.5
  - Rounded-up to the nearest whole number, each read will need 1 x Write Capacity Unit per read operation
  - Multiplied by the number of reads per second = 100 Write Capacity Units required

- DynamoDB Provisioned Throughput Exam Tips
  - Provisioned Throughput is measured in Capacity Units
  - 1 x Write Capacity Unit = 1 x 1KB Write per second
  - 1 x Read Capacity Unit = 1 x 4KB Strongly Consistent Read OR 2 x 4KB Eventually Consistent v Reads per second

### DynamoDB Accelerator - DAX

- What is DAX?
  - DynamoDB Accelerator (or DAX) is a fully managed, clustered in-memory cache for DynamoDB
  - Delivers up to a 10x read performance improvement
  - Microsecond performance for millions of requests per second
  - Ideal for Read-Heavy and bursty workloads
  - e.g. auction applications, gaming and retail sites during black Friday promotions

- How does it work?
  - DAX is a write-through caching service - this means data is written to the cache as well as the back end store at the same time
  - Allows you point your DynamoDB API calls at the DAX cluster
  - If the item you are querying is in the cache (cache hit), DAX returns the result to the application
  - If the item is not available (cache miss) then DAX performs an Eventually Consistent GetItem operation against DynamoDB
  - Retrieval of data from DAX reduces the read load on DynamoDB tables
  - May be able to reduce Provisioned Read Capacity

- What is NOT suitable For?
  - Caters for Eventually Consistent reads only - so not suitable for applications that require Strongly Consistent reads
  - Write intensive applications
  - Applications that do not perform many read operations
  - Applications that do not require microsecond response times

- DAX Exam Tips
  - Provides in-memory caching for DynamoDB tables
  - Improves response times for Eventually Consistent reads only
  - You point your API calls the DAX cluster, instead of your table
  - If the item you are querying is on the cache, DAX will return it; otherwise it will perform an Eventually Consistent GetItem operation to your DynamoDB table
  - Not suitable for write-intensive applications or applications that require Strongly Consistent reads
