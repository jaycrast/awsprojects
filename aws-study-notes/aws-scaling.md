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
        - +8 free IPs needed per subnet for ELB nodes, /27 or larger to allow for scale
        - Internal LB would be used to connect your multi tier environment to eachother
            - client would communicate with internet-facing LB which points him to a webserver, webserver then goes through an internal LB to   an application instance, and the application server reaches out to the database server. This allows your infrastructure to scale indepedently of eachother
        - Crozz-Zone Load Balancing
            - the ability to distribute load across zones. LB Node B can distribute load to to instances inside AZ A
     - ALB
        - Layer 7 LB
        - rules direct connection which arrive at the listener
        - can look at content type, cookies, customer headers, user location etc
        - HTTP/s always terminated on the ALB, no unbroken SSL
            - a new connection is then made to the application, no end to end unbroken encryption
    - NLB
        - Layer 4 LB, tcp/tls/udp
        - no visibility or understanding of HTTPs
        - no headers, cookies or session stickiness
        - extremely fast, millions of request per second, 25% of alb latency
        - use this for SMTP, SSH, game server, financial apps that are not http/s
        - health checks just check ICMP/TCP handshake, and are not app aware
        - NLBs can have static IPs, useful for whitelisting
        - can forward TCP straight to instances, unbroken encryption
        - used with private link to provide services to other VPCs

- Launch Template and Launch Configuration
    - Allow you to define the config of an EC2 instance in advance
    - AMI, instance type, storage and key pair
    - networking and security groups
    - userdata and IAM Role
    - both are NOT editable once defined, Launch Templates have versions
    - LT provides newer features, including T2/T3 unlimited, placement groups, capacity reservations, elastic graphics
    - these templates are used for auto scaling groups (ASG)
        - LT's can also be used to directly launch EC2 instances from the console UI/CLI

- Auto Scaling Groups
    - automatic scaling and self healing for EC2
    - uses launch templates or configurations
    - has a minimum, desired and maximum size
        - keeps the running instances at the desired capacity by provisioning or teminating instances
    - scaling plicies automate based on metrics
        - manual scaling, manually adjust desired capacity
        - scheduled scaling, time based asjustment for sales, slow periods etc
        - dynamic scaling
            - simple "cpu above 50% +1, cpu below 50% -1"
            - stepped scaling, bigger +/- based on difference
            - target tracking, desired aggregate cpu = 40%
        - cooldown periods between scaling policies
    - Scaling Processes
        - launch and terminate, suspend and resume
        - AddToLoadBalancer, add to LB on launch
        - AlarmNotification, accept notification from CloudWatch
        - AZRebalance, balances instances evenly across all of the AZs
        - HealthCheck, instance checks on/off
        - ReplaceUnhealthy, terminate unhealthy and replace
        - ScheduledActions, scheduled on/off
        - Standy, use this for instances of InService vs Standby for maintenance
        - ASG is free, only billed for the resources created
        - use cooldowns to avoid rapid scaling

- ASG Lifecycle Hooks
    - Custom actions on instance during ASG actions
        - instance launch or instance terminate transitions
    - instances are paused within the flow, they wait until timeout. then either continue or abandon
    - or you can resume the ASG process CompleteLifecycleAction
    - configured with EventBridge or SNS Notifications
    

- SSL Offload
    - Bridging
        - Default mode of an Application Load Balancer
        - one or more clients make one or more connections to an ELB
        - SSL connection occurs between client and node, LB needs SSL cert that matches the domain name of the application
        - the session is terminated at the ELB, and ELB initiated a new SSL connection to the backend. EC2 needs matching SSL cert for domain
    - Pass-through
        - mode used for Network Load Balancer
        - no encryption or decryption happens on the NLB, connection is passed directly to the backend instance
        - Each instance still needs to have the appropriate SSL cert installed, no certificate exposure to AWS, all self managed
    - Offload
        - Listener is configured for HTTPS. Connection and terminated and then passed to backend connections using HTTP
        - ELB to instance connections use http, no SSL certificate or cryptographic requirements

- Connection Stickiness
    - stickiness generates a cookie which locks the device to a single backend instance for a duration
    - cookies should usually be stored away from the instance so that you dont run in to uneven load issues

- Gateway Load Balancer
    - helps you run and scale 3rd party appliances
        - firewalls, intrusion detection and prevention systems
        - inbound and outbound traffic inspections
    - GWLB endpoints, traffic enters and leaves via these end points
    - the GWLB balances across multiple backend appliances
    