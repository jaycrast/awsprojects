- Advanced VPC Networking

- VPC Flow Logs
    - Capture metadata (NOT CONTENTS)
        - source IP, destination IP, ports
    - sensors are attached to a VPC, all ENIs in that VPC
        - can be applied to just a subnet, or to ENIs directly
    - flow logs are NOT realtime
    - can be configured to go to S3 or CloudWatch Logs
    - if S3 they can be integrated with a 3rd party tool for logging
    - can use Athena for querying
    - can be configured to capture accepted, rejected, or ALL metadata
  
  - Egress Only Internet Gateway
    - only allows connections from inside a VPC to outside
    - NAT allows private IPs to access public networks without allowing externally initiated connections
    - IPv6 IG allows all IPs in and out because all IPv6 addresses are public IPs
    - Egress-Only is built to solve this problem for IPv6 instances

- VPC Endpoints
    - Gateway Endpoints
        - provide private access to S3 and DynamoDB
            - allows access without a public IP to private aws services
        - preflix list added to route table
        - HA across all AZs in a region by default
        - endpoint policy is used to control what it can access
        - Gateway Endpoints are regional, can't access cross-region services
        - prevent leaky buckets, S3 buckets can be set to private only by allowing access ONLY from gateway endpoint
    - Interface Endpoints
        - just like Gateway Endpoints, provide private access to AWS public services
        - historically used for private access for anything NOT S3 and DynamoDB, but S3 is now supported
        - difference is interface endpoints are not HA by default, added to specific subnets inside that VPC
        - network access controlled by security groups
        - can still use endpoint policies to restrict what can be done with the endpoint
        - used for TCP and IPv4 ONLY
        - uses PrivateLink, allows external services to connect to your VPC
        - instead of a preflix list in a route table, they use DNS

- VPC Peering
    - direct encrypted network link between two VPCs, no more than two
    - can be cross-region, within the same account or between two AWS accounts
    - optional public hostnames resolve to private IPs can be enabled
    - same region SGs can reference peer SGs, cross region will need to reference the IP
    - VPC peering does not support transitive peering, if A connects to B, and B connects to C, A does not peer to C
    - Routing Configuration is needed, SGs and NACLs can filter

