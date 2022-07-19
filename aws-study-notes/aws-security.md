- AWS Secrets Manager
    - shares functionality with Parameter Store
    - designed specifically for passwords and API keys
    - usable via console, cli, api or SDKs
    - supports automatic rotation using Lambda
    - directly integrates with some AWS products like RDS

- AWS Shield
    - provides protection against DDoS for AWS resources
    - two versions, shield standard and shield advanced
    - standard is free and comes with Route53 and CloudFront
    - advanced costs $3000 per month and includes EC2, ELB, Global Accelerator
    - Protection against both layer 3 and layer 4 DDoS attacks
    - DDoS Response Team and Financial Insurance, covers increased costs of elastic resources created from an attack
    
- AWS WAF
    - Layer 7 (HTTP/s) Firewall
    - Protects against complex layer 7 attacks or exploits
    - SQL Injections, cross site scripting, Geo Blocks, Rate Awanress
    - WEBACL integrated with app load balancer, API Gateway, CloudFront
    - rules are added to WEBACL and evaluated when traffic arrives

- CloudHSM
    - similar to KMS, but its a true single tenant hardware security module
    - AWS provisions it, but its fully customer manages after that
    - Fully FIPS 140-2 Level 3 compliant (KMS is L2 overall)
    - Industry Standard APIs - PKCS#11, JCE, CryptoNG
    - KMS can use CloudHSM as a custom key store, CloudHSM integration with KMS
    - not HA, needs to be built in a cluster to be highly available
    - CloudHSM client needs to be installed on EC2 instance to access HSM cluster
    - AWS has NO access to the HSM apart from maintenance, no access to secure area where keys are held
    - no native AWS integration with S3 service side encryption
    - can offload SSL/TLS processing for web servers
    - enable transparent data encryption for oracle databases
    - protect the private keys for an issuing certificate authority

- AWS Config
    - Record config changes over time on resources
    - great for auditing of changes, compliance with standards
    - does not prevent anything, no protection. just records activities in AWS
    - regional service, supports cross region and account aggregation
    - changes can generate SNS notifications and near-realtime events via EventBridge and Lambda
    - once enabled, configs are recorded and stored in the Config S3 Bucket
    - Config Rules can be configured and when resources are evaluated against the rules they are either compliant or non-compliant
    - config can be sent to EventBridge, which can invoke a Lambda function based on an event for automatic remediation

- Amazon Macie
    - Data Security and Privacy Service
    - Discover, monitor and protect data stored in S3 buckets
    - automated discovery of data, PII, PHI, Finance
    - Managed Data Identifiers, built-in, ML/Patterns
    - Custom Data Identifiers, Proprietary, regex based
    - integrates with security hub and finding events to eventbridge

- AWS Inspector
    - scans EC2 instances and the instance OS
    - checks for vulnerabilities and deviations against best practice
    - provided with a report of findings ordered by priority
    - Network Assessment can be ran without an agent
    - Network and Host Assessment requires the Inspector Agent to be installed on the instance
    - Rule packages determine what is checked
    - Check reachability end to end. EC2, ALB, DX, ELB, ENI, IGW, ACLs, RTs, SGs, Subnets, VPCs, VGWs and VPC Peering
    - Checks common vulnerabilities and exposures (CVE)
    - Center for Internet Security Benchmarks (CIS)
    - Checks AWS Security Best Practices for Amazon Inspector as well


- AWS GuardDuty
    - Continuous security monitoring service
    - analyses supported data sources
    - uses AI/ML and threat intelligence feeds
    - identifies unexpected and unauthorized activity
    - can notify (SNS), or invoke functions from events (Lambda/EventBridge) for protection/remediation
    - supports multiple accounts, master and member
    