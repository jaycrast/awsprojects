- Storage
    - Direct (local) attached storage is storage on the EC2 Host
    - EBS is network attached storage, volumes delivered over network
        - highly resilient, separate from the instace hardware and survives issues from the EC2 host
    - Ephemeral Storage is temporary storage (Direct is ephemeral)
    - Persistent Storage lives on past the lifetime of the EC2 instance (EBS is persistent)

    - Block Storage is volume storage presented to the OS as a collection of blocks, no structure provided
        - mountable and bootable
    - File Storage is presented as a file share and has structure, mountable but NOT bootable
    - Object Storage is a flat collection of objects, not mountable and not bootable (S3)
    
- Storage Performance
    - IO (block) size 
    - IOPS (input output operations per second)
    - Throughput (speed)
    - IO x IOPS = Throughput in mbps

- EBS (elastic block store)
    - takes raw physical disks and allocates then presents them as volumes
        - can be encrypted with KMS
    - instances see block device and then create a file system on the disk
    - EBS is provisioned in ONLY ONE AZ
    - usually attached to only one EC2 instance, but can be mutli attached for cluster resources
    - can be detached and reattached, and is linked to the EC2 instance so will follow of instance moves between hosts
    - Snapshot backups can be taken and stored in S3 so they have regional resiliency now, not just AZ resilient
    - Different physical storage types, sizes and performance profiles
    - Billed based on GB/month

    - GP2 is general purpose SSD
        - high performance storage for a low price
        - great for boot volumes, low latency interaction of applications, dev test environments
        - GP2 is currently the default
        - uses an IO bucket that wills with min 100 IO credits per second
            - additional 3 IO credits per 1GB of storage
            - 3k bust iops
    - GP3 general purpose SSD
        - 3k IOPS and 125 MiB/s STANDARD
        - GP3 is 20% cheaper vs GP2, and you can pay an extra cost for up to 16k IOPS and 1k MiB/s

    - Provisioned IOPS SSD (io1/2)
        - max 64k iops per volume, 1k MB/s
        - block express is 256k IOPS per volume, 4k MB/s
        - 4GB-16TB io1/2, 4GB-64TB for block express
        - they have a per instance performance MAX, to achieve this max, you would need to use multiple volumes per instance
        - used normally with smaller volumes that need extremely high/fast performance, and super low latency for sensitive workloads

    - Hard Disk Drive
        - ST1 is faster, throughput optimized
            - cheap, good for large data that, is frequent access that is sequential
            - 125gb to 16TB max, max 500 IOPS (1MB blocks) so that's still 500MB/s max
        - SC1 is slower, cold HDD
            - infrequent workloads, maximum economy, store maximum data and don't care about performance
            - 250 iops, 250 MB/s max

- Instance Store Volumes
    - block storage devices
    - physically connected to ONE ec2 host
        - instances which are on that host can access this isntance store volume
        - highest storage performance in AWS
        - included in the instance price
        - attached AT launch
        - data is temporary, and is lost if instance moves between hosts
            - DO NOT use for permenant data
        - significantly faster than EBS storage
        