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

!!EXAM!!
- bucket names are globally unique
- 3-63 characters, all lowercase, no underscores
- start with lowercase letter or a number
- can't be IP formatted
- bucket limits - 100 soft limit, 1000 hard limit per AWS account
    - take a single bucket, and divide that one bucket between multiple users using prefix object names
- unlimited objects in bucket, 0 bytes to 5TB
- Key = Name, Value = Data