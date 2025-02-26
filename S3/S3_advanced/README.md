Amazon S3 – Moving between Storage Classes:

You can transition objects between storage classes 
For infrequently accessed object, move them to Standard IA
For archive objects that you don’t need fast access to, move them to Glacier or Glacier Deep Archive
Moving objects can be automated using a Lifecycle Rules
![alt text](image/image8.png)


Lifecycle Rules:
Transition Actions – configure objects to transition to another storage class
Move objects to Standard IA class 60 days after creation
Move to Glacier for archiving after 6 months
Expiration actions – configure objects to expire (delete) after some time
Access log files can be set to delete after a 365 days
Can be used to delete old versions of files (if versioning is enabled)
Can be used to delete incomplete Multi-Part uploads
Rules can be created for a certain prefix (example: s3://mybucket/mp3/*)
Rules can be created for certain objects Tags (example: Department: Finance)

Requester pays
In general, bucket owners pay for all Amazon S3 storage and data transfer costs associated with their bucket
With Requester Pays buckets, the requester instead of the bucket owner pays the cost of the request and the data download from the bucket
Helpful when you want to share large datasets with other accounts
The requester must be authenticated in AWS (cannot be anonymous)
![alt text](image/image9.png)

Event notification:
S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…
Object name filtering possible (*.jpg)
Use case: generate thumbnails of images uploaded to S3
Can create as many “S3 events” as desired
S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer
![alt text](image/image10.png)
![alt text](image/image11.png)

Base_line performance
Amazon S3 automatically scales to high request rates, latency 100-200 ms
Your application can achieve at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix in a bucket. 
There are no limits to the number of prefixes in a bucket. 
Example (object path => prefix):
bucket/folder1/sub1/file => /folder1/sub1/
bucket/folder1/sub2/file => /folder1/sub2/
bucket/1/file => /1/
bucket/2/file => /2/
If you spread reads across all four prefixes evenly, you can achieve 22,000 requests per second for GET and HEAD


Multi-Part upload: 
recommended for files > 100MB, must use for files > 5GB
Can help parallelize uploads (speed up transfers)
![alt text](image/image12.png)

S3 Transfer Acceleration 
Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region 
Compatible with multi-part upload
![alt text](image/image13.png)

S3:Batch operations
Perform bulk operations on existing S3 objects with a single request, example:
Modify object metadata & properties, Copy objects between S3 buckets
Encrypt un-encrypted objects, 
Modify ACLs, tags
Restore objects from S3 Glacier
Invoke Lambda function to perform custom action on each object
A job consists of a list of objects, the action to perform, and optional parameters
S3 Batch Operations manages retries, tracks progress, sends completion notifications, generate reports …
You can use S3 Inventory to get object list and use S3 Select to filter your objects
![alt text](image/image14.png)


Storage lens:
Understand, analyze, and optimize storage across entire AWS Organization
Discover anomalies, identify cost efficiencies, and apply data protection best practices across entire AWS Organization (30 days usage & activity metrics)
Aggregate data for Organization, specific accounts, regions, buckets, or prefixes
Default dashboard or create your own dashboards
Can be configured to export metrics daily to an S3 bucket (CSV, Parquet)
![alt text](image/image15.png)


storage lens: metrics:
1. Summary Metrics
General insights about your S3 storage
StorageBytes, ObjectCount…
Use cases: identify the fastest-growing (or not used) buckets and prefixes

2. Cost-Optimization Metrics
Provide insights to manage and optimize your storage costs
NonCurrentVersionStorageBytes, IncompleteMultipartUploadStorageBytes…
Use cases: identify buckets with incomplete multipart uploaded older than 7 days, Identify which objects could be transitioned to lower-cost storage class

3. Data-Protection Metrics
Provide insights for data protection features
VersioningEnabledBucketCount, MFADeleteEnabledBucketCount, SSEKMSEnabledBucketCount, CrossRegionReplicationRuleCount…
Use cases: identify buckets that aren’t following data-protection best practices

4. Access-management Metrics
Provide insights for S3 Object Ownership
ObjectOwnershipBucketOwnerEnforcedBucketCount…
Use cases: identify which Object Ownership settings your buckets use

5. Event Metrics
Provide insights for S3 Event Notifications
EventNotificationEnabledBucketCount (identify which buckets have S3 Event Notifications configured)

6. Performance Metrics
Provide insights for S3 Transfer Acceleration
TransferAccelerationEnabledBucketCount (identify which buckets have S3 Transfer Acceleration enabled)

7. Activity Metrics
Provide insights about how your storage is requested
AllRequests, GetRequests, PutRequests, ListRequests, BytesDownloaded…
Detailed Status Code Metrics
Provide insights for HTTP status codes
200OKStatusCount, 403ForbiddenErrorCount, 404NotFoundErrorCount

8. Free Metrics
Automatically available for all customers
Contains around 28 usage metrics
Data is available for queries for 14 days

9. Advanced Metrics and Recommendations
Additional paid metrics and features
Advanced Metrics – Activity, Advanced Cost Optimization, Advanced Data Protection, Status Code
CloudWatch Publishing – Access metrics in CloudWatch without additional charges
Prefix Aggregation – Collect metrics at the prefix level
Data is available for queries for 15 months

