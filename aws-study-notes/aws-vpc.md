- VPC
    - Need to decide what size the VPC should be ahead of time
    - Are there any Networks we can't use
    - Think about the other VPC's already in your environment, cloud, on prem, partners and vendors
    - Try to predict the future
    - VPC strucutre, tiers and availability zones

    - Based off our fictional business, here's what we know so far
        - 3 major offices that will need a connection, London, New York and Seattle. We also have field workers that will need connection
        - On-prem: 192.168.10.0/24, AWS Pilot: 10.0.0.0/16, Azure Pilot: 172.31.0.0/16, so we can't use any of those ranges
            - 192.168.15.0/24 used by London
            - 192.168.20.0/25 used by New York
            - 192.168.25.0/24 used by Seattle
            - Our provider told us google cloud is using the default range os 10.128.0.0/9 (10.128.0.0 - 10.255.255.255)
        - VPC minimum is /28 (10.0.0.0/28 is 10.0.0.1 - 10.0.0.15)
        - VPC maximum is /16
        - Think of the highest possible regions the business will operate in, then add a few as a buffer
            - Reserve 2+ networks per region being used per account to account for growth - assume 4 accounts
                - This is 40 ranges
                - starting at 10.16, can't use 10.128
                    - this gives us 10.16 to 10.127 which is plenty
        - .0 is reserved for the network
        - .1 is reserved for VPC router
        - .2 is reserved for VPC DNS
        - .3 is reserved for future use
        - .255 reserved for broadcast

        - VPC router is HA device, both default and custom
        - runs in all AZs the VPC uses
        - network +1 address
        - routes traffic between subnets in the VPC
        - controlled by route tables which control what it does with the traffic
        - VPC has a main route table by default

        - Intergateway (IGW) is region resilient and is attached to a VPC
        - VPC can have 0 IGW which is private, or it can have only 1 IGW which makes it available to all AZ's the VPC is in
            - IGW gateways traffic between the VPC and the internet or AWS public zones
            - managed, AWS handles the performance
        - IGW would be a target in the route table of the VPC router
        - with an IGW, the subnet is then considered public
        - IGW changes the IP address of the source to it's public IP, then forwards it to the destination address

- Stateful vs Stateless Firewalls
    - Stateless
        - Sees the inbound and outbound traffic as two different connections
        - Which means you would need 2 firewall rules, 1 for inbound and 1 for outbound for that specific connection  
    - Stateful
        - Stateful firewall is intelligent enough to see that request and response of a connection are related
        - You only have to specify the request, and the response is automatically allowed if the request is allowed  
            - Less firewall rules, less chance for mistakes, dont need to allow the full range of ephemeral ports for better security

- Network Access Control Lists (NACL)
    - NACLs are stateless, need both inbound and outbound rules
    - Connection WITHIN subnets aren't impacted by NACLs
    - NACLs filter traffic crossing the subnet bounadry both inbound and outbound
    - Rules are processed in order, lowest rule number first. once a match occurs, processing stops
        - * is an implicit deny if nothing else matches
    - NO logical resources, IPs/CIR, ports and protocols only
    - CANT be assigned to AWS resouces, only subnets
    - Used together with Security Groups to add explicit DENY
    - Each subnet can have one NACL, but many subnets can use the same NACL

- Security Groups
    - SGs are stateful
        - they detect response traffic automatically
    - Allowed in or out request = allowed response
    - There is NO explicit deny, so you'd have to use a NACL to explicity deny a single bad actor
    - They operate above NACL on the OSI layer
        - they support IP/CIDR/ports, but also logical resources
            - this includes other security groups and itself
    - Attached to Elastic Network Interfaces (ENI's), they are not attached to instances or subnets

- Network Address Translation (NAT)
    - set of processes that can remap the source or destination IP
    - IP masquerading is how you can assign one public IP to many private IP's, only works for outgoing internet access
    - you can use an EC2 instance configured as a NAT device, or you can now use the NAT Gateway Service in VPC
    - Elastic IPs (static IPs) are used for NAT Gateways
    - NATGW's are AZ resilient
        - need to deploy 1x NATGW and 1x route table in each AZ
    - Scaled to 45GBps, $ duration and data volume
    - you can deploy more gateways to increase your bandwidth 