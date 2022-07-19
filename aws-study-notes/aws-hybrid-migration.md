- Hybrid Environments and Migration

- BGP
    - Autonomous System, routers controlled by one entity, a network in BGP
    - ASN are unique and allocated by IANA
    - operates over TCP on port 179 and its reliable
    - not automatic, peering is manually configured
    - BGP is a path-vector protocol it exchanges the best path to a destination between peers, path is called the ASPATH
    - iBGP is internal BGP, routing within an AS
    - eBGP is external BGP, routing between AS's
    - ASPATH Prepending can be used to artifically make a satellite path look longer than it actually is, making a fibre path with more hops the preferred path

- Site to Site VPN
    - a logical connection between a VPC and on-premises network encrypted using IPSec, running over public internet
    - full HA, if you design and implement it correctly
    - quick to provision, less than an hour
    - Virtual Private Gateway (VGW)
    - Customer Gateway (CGW)
    - VPN connection between the VGW and CGW
- Considerations
    - 1.25GBps speed limit, if you need faster speeds, don't use site to site VPN
    - inconsistent latency since it's over the public internet
    - Cost, AWS hourly cost, GB out cost, data cap
    - speed of setup is in hours, all software configuration
    - can be used as a backup for Direct Connect

- Direct Connect (DX)
    - 1gb or 10gb network port into AWS
    - AWS gives you the port at a DX location, and you need to connect your customer router to that part
        - small businesses can use a partner router to extend to your local location
    - multiple virtual interfaces (VIFS) over one DX
    - private VIF (VPC), and public VIF (public AWS services, s3 etc)
    - direct connect is NOT encrypted by default
    - you can use an IPSEC VPN over the public VIF pointing at private gateway endpoints to get encryption

- Transit Gateway (TGW)
    - Network Transit Hub connects VPC's to on-prem networks
    - significantly reduces network complexity
    - Single network object, HA and scalable
    - Attachments to other network types
    - VPC, site to site VPN, and direct connect gateway
    - AWS RAM allows you to share products or services between AWS accounts
    - can peer with different regions or accounts

- Storage Gateway
    - Hybrid Storage Virtual Appliance is configured on prem
    - extension of file and volume storage into AWS
    - allows you to keep volume storage locally with backups in AWS, or migrate existing tape backups in to AWS
    - trickle migrations of existing infrastructure in to AWS
    
    - 3 modes to select from at creation
        - Tape Gateway (VTL)
            - virtual tapes in to S3 and Glacier
        - File Gateway, for SMB and NFS
            - file storage backed by S3 objects
        - Volume Gateway (Gateway Cache/Stored) over iSCSI
            - Block storage backed by S3 and EBS Snapshots

- AWS DataSync
    - Data Transfer service TO and FROM AWS
    - used for migrations, data processing transfers, archival, cost effective storage, or DR
    - designed to work at huge scale
    - keeps file metadata (permissions and timestamps)
    - built in data validation
    - 10GBps per agent (100tb per day)
    - bandwidth limiters, avoid link saturation
    - incremental and scheduled transfer options
    - compression and encryption
    - automatic recovery from transit errors
    - aws service intrgration (S3, EFS, FSx)
    - pay as you use, per GB
    - agent is installed locally on something like VMWare

- FSx for Windows
    - provides fully managed native windows file servers and file shares
    - designed for integration with windows environments
    - integrates with directory service or self managed AD
    - deployed in single or multi-AZ within a VPC
    - on demand and scheduled backups
    - accessible using VPC, peering, VPN and DX

- FSx for Lustre
    - Designed for high performance computing, linux clients (POSIX)
    - Machine Learning, Big Data, Financial Modelling
    - 100s GB/s throughput and sub millisecond latency
    - Two modes, scratch and persistent
        - Scratch is highly optimized for short term, no replication and fast
        - Persistent is longer term, resilient and self healing
    