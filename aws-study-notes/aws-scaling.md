- HA and Scaling

- Regional and Global AWS Architecture
    - Global Service Location and Discovery
    - Content Delivery (CDN) and optimization 
    - Global health checks and failover

- Evolution of Elastic Load Balancers (ELB)
    - three types of ELB in AWS
        - V1 Classic Load Balancer (CLB), V2 Application Load Balancer (ALB), and V2 Network Load Balancer (NLB)
        - V1 isn't really layer 7, lacks features, and only supports 1 SSL cert per CLB
        - ALB supports http/s, websocket
        - NLB supports TCP, TLS, UDP and any other custom protocols not supported by ALB

    - ELB Architecture
        - configured to run in more than 1 AZ
            - each AZ gets it's own LB node and scale with load
        - Nodes are configured with listeners which accept traffic on a port and protocol and communicate with target on port/protocol
        - Internet-facing nodes get a public IP
        - Internet- facing LB can access both public AND private EC2 instances