S3 Buckets
- Global Storage Platform - regional based/resilient
- public service, unlimited data and multi-user
- infinitely scalable
- accessed via gui, CLI, API, HTTP

S3 is categorized in to objects and buckets
- Objects
    - two main components 
        - key (file name): koala.jpg. If you know the key and the bucket, you can access the object
        - value = the content being stored (picture)
    - zero bytes to 5TB in size per object
- Buckets are containers
    - buckets never leave the region unless you configure it to
    - blast radius is the entire region
    - bucket is identified by its name
        - needs to be globally unique across all regions and AWS accounts
    - buckets can hold an unlimited amount of objects
    - no complex structure, flat. all objects are stored at the same level
    - folders are prefix's in object names (/folder/koala1.jpg)

- S3 is an object store, not a file or block storage system
    - if you are accessing the whole of an object like an image or audio, you can use S3
    - if you have a windows system that needs a file share, you can't use S3 since S3 is flat
    - can't mount S3 bucket, so no block storage for servers
- Great for large scale data storage, distribution or upload
- Great for offload
- Input and/or Output to many AWS products
- ARN Amazon Resource Name

S3 Bucket Policies
- a form of resource policy
- like identity policies but attached to a bucket
- resource perspective permissions
- allow/deny same or different accounts
- allow/deny anonymous principals
    - can allow access to the s3 bucket outside of your organization
- principal is who the resource applies to, identity policies don't have a principal because the principal is assumed by the identity
- conditions are used to allow/deny access to the resource unless the condition is matched
    - NotIPAddress, if your ip doesn't match the ip used in the condition, you're denied access
- ACL's are legacy for both objects and bucket
- block public access applies REGARDLESS of the what resource policy is

S3 Static Website Hosting
- normal access is via AWS APIs
- this feature allows access via HTTP
- index and error documents are set
- website endpoint is created
- bucket name MUST match domain name
- EC2 instance would run the webserver, html would be served, and that html could then point to an s3 static website bucket to pull static images
- static websites could be used in S3 for maintenance landing pages instead of using another EC2 instance
- Route53 can be used to create an A record to point to the bucket website endpoint address

S3 Object Versioning
- A bucket can go from disabled to enabled
- A bucket CANNOT go from enabled to disabled
    - But a bucket can go from enabled to suspended, and then back to enabled
- versioning lets you store multiple version of an object in a bucket
    - if an object is stored with the same name, bucket versioning generates a new version
- version disabled objects have an ID of null
    - version enabled gives the object an ID associated with the key (object name dog.jpg)
    - current version is the latest version uploaded
- if we delete an object and we dont specify the ID, it gives the object a delete marker
    - delete marker is a special version of the object that hides all previous versions of the object
        - you can delete the delete marker, which unhides the object
- to actually delete an object, you need to specify the object ID
- space is consumed by ALL versions

- MFA is required to change bucket versioning state, from enabled to suspended
- MFA is required to delete versions
    - MFA + code passed with api calls

S3 Performance


!!EXAM!!
- bucket names are globally unique
- 3-63 characters, all lowercase, no underscores
- start with lowercase letter or a number
- can't be IP formatted
- bucket limits - 100 soft limit, 1000 hard limit per AWS account
    - take a single bucket, and divide that one bucket between multiple users using prefix object names
- unlimited objects in bucket, 0 bytes to 5TB
- Key = Name, Value = Data
