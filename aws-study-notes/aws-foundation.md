	- AWS Certified Solutions Architect Associate -
	
Company Background:
1. Animal Rescue
2. 100+ staff
3. Call center, Admin, IT, Marketing, Legal/Accounting
4. 100 remote workers across Australia and Globally
5. Offices in London and US
6. On-premise datacenter in Brisbane (5 racks)
7. Badly implemented AWS trial in Sydney
8. Cost-concious but progressive

On-prem: 192.168.10.0/24
AWS Pilot: 10.0.0.0/16
Azure Pilot: 172.31.0.0/16

Problems with current infrastructer:
1. Legacy on-prem hardware is failing
2. AWS/Azure Pilots were messy, and didn't have any cost reduction/no improvement over on-prem solutions
3. Performance issues, field workers across the world using Brisbane infrastructure
4. Lack of HA/Scalability
5. Lack of automation skills from small IT team

Ideal Outcomes:
1. Fast performance for field workers
2. Able to deploy into new regions quickly when required
3. Infrastructure can be provisioned and deprovisioned to meet the business needs
4. Low cost and scalable
5. AGILITY
6. Automation, low base staffing costs


IAM:

Production IAM Sign-in: https://jc-aws-production.signin.aws.amazon.com/console
General IAM Sign-in: https://jc-aws-gen.signin.aws.amazon.com/console

- Every AWS account comes with its own running copy/database of IAM. 
	- IAM is a globally resilient server so any data is always secure across all AWS regions
	- What you see in IAM is specific to your account. IAM has full trust between itself and the AWS Account
- Lets you create three different types of idenity users (users, groups, roles)
- Roles are used by AWS services or for granting external access to your AWS account
	- Giving an uncertain number of things or services access, such as giving EC2 instances access to all S3 buckets
- Policies are used to allow or deny access to AWS services only when they are attached to users groups or roles

- Identity Provider (IDP)
- Authenticate (security principal to verify the identity of the user)
- Authorize, allow or deny access

- Global service, one global DB for the account and is reilient. 
- Has no cost, but has a limit on users
- No direct control on external accounts or users
- IAM uses identity federation (Active Directory, google, facebook) and MFA

- Long term credentials are username and password for the user, or AccessKeys
	- IAM user has 1 username and 1 password
		- Credentials contain a public and private part, username being public and password being private
- An IAM user can have two access keys
	- keys can be created, deleted, active or inactive
- Made of two parts, access key ID and secret access key
	- secret access key needs to be safely stored as you can't access it again
- You use both of these parts when used to sign the request from an API/CLI or another service to AWS
	- secret access key is like the password, and access key ID is like the username
	- if access keys are compromised, you'd need to delete both parts of the key and generate a new one


Cloud Computing:

1. On-Demand Self-Service (provision and terminiate using ui/cli without human interaction)
2. Broad Network Access (access services over an networks, on any devices using standard protocols and methods)
3. Resource Pooling (economies of scale, cheaper service)
4. Rapid Elasticity (scale up or down automatically in response to system load)
5. Measured Service  (usage is measured, only pay for what you consume)

Public vs Private vs Hybrid vs Multi Cloud:
- AWS, Azure, Google are public clouds that reach all 5 standards of cloud computing

- Multi Cloud is using multiple cloud environments (using both AWS and Azure)
		- mirrored environments provide cloud level redundency 

- AWS Outposts, Azure Stack, Google Anthos are private clouds
	- cloud computing that meets the 5 standards but are dedicated to just your business. Not similar to hyper-v, vmware.
	
- Hybrid Cloud is only when you use a private and public cloud together, it's not hybrid if you use public cloud and on-prem infrastructure 

Cloud Service Models (X as a Service):

Infrastructure Stack - application > data > runtime > container > O/S > vritualization > servers > infrastructure > facilities
- parts of this you manage, and other parts managed by the vendor
- unit of consumption. In AWS if you create a veritual machine, you consume the operating system
- On-Prem you own and control the entire infrastructure stack
	- DC Hosted, you control you entire infrastructure stack except for the facilities. 
	
Infrastructure as a Service (IaaS)
- in a cloud environment, you own/control the O/S and up. Facilities, infra, servers and virtualization are controlled by the provider (EC2)
- IaaS, you consume the O/S and that is what you are billed on

Platform as a Service (PaaS)
- aimed more so at developers who have an application they want to run and dont care about the infrastructure
	- if you run a python application, you pay for the python runtime environment
	- you give the provider the application, they host it, and you control the run time of that application
- you consume the runtime

Software as a Service (SaaS)
- consuming an application with no control or overhead costs by using the application
	- consuming Netflix, Outlook, etc.
- you consume the application

YAML (YAML aint markup language) CloudFormation Templates:

- human readable data serialization language
	- unordered collection of key:value pairs, each key has a value
- cat1: roffle, cat2: truffles - these keys and values are strings
- YAML also supports lists
	mycats: ["roffle, truffles"]
	
	mycats:
		- "roffle"
		- truffles"
		
- indentation matters in YAML, must be the same spaces in the lists

- Dictionary
	- a mix of key value pairs and dictionaries
	
	mycats:
		-	name: roffle
			color: [black, white]
		-	name: truffles
			color: "mixed"
		- name: penny
			color: white
			
		Resources:
			s3bucket:
				Type: "AWS::S3::Bucket"
				Properties:
					BucketName: "ac1337catpics"
					
JSON (JavaScript Object Notation)
	- lightweight data-interchance format. easy for humans to read/write and machines to parse and generate
	- doesn't care about indentations like YAML, easier to write.
- object, unordered set of key: value pairs enclosed by { }
- array is ordered collection of values separated by commas and enclosed in [ ] like a YAML lists

{
	"cats": ["roffle, truffle"]
	"colors": [mixed, black"]
	"size": [small, large"]
}

{
	"Resources": {
		"s3bucket": {
			"Type": "AWS::S3::BUCKET" ,
			"Properties: {
				"BucketName": "ac1337catpics"
			}
		}
	}
}


Networking Starter Pack 0:
- Local Networking (ethernet)
- Routing 
- Segmenting, Ports, and Sessions
- Applications
- OSI 7-Layer Models

Layer7- Application
Layer6 - Presentation
Layer5 - Session
Layer4- Transport
Layer3 - Network
Layer2 - Data Link
Later1 - Physical

Media Layers - Layers 1 through 3
Host Layers - Layers 4 through 7

LAYER 1:
Physical - network interface. physical cable (copper, fibre, wifi) used to transmit elictrical signal between 2 or more devices.
- raw bit streams between devices, transmitting 1's and 0's 
- layer 1 device understands only level 1, level 3 device understands layer 1, 2 and 3
- layer 1 physical hub allow more connections then a single point to point
	- hubs transport anything received to every other port on the hub
	- no device addressing on a layer 1 hub, all data processed by all devices
	- collision corruption if more than 1 device transmits on the same layer 1 medium
	- no media access control and no collision detection

LAYER 2:
Data Link - supports the transfer of data between devices
- layer 2 REQUIRES a functional layer 1 network (copper, fiber, wifi)
- frames are a format for sending information over a layer 2 network
	- this is how endpoints get MAC addresses
	- frames can be addressed to a destination or broadcast
	- layer 2 transmits the frames via layer 1. layer 1 doesn't understand the frame, just passes it to other connected devices
FRAME:
- preamble is the start of the frame, and start frame delimiter, 56 bits. SFD - 8 bits
- Destination MAC address
- Source MAC address
- Ether type (which layer 3 protocol is putting it's data inside the frame, such as IP)
	- this is known as the MAC header
- Payload 46-1500 bytes (payload is the data the frame is carrying from source to destination)
- FCS 32 bits (frame check sequence). lets the destination check if the frame is corrupted when it's received

- CSMA (carrier sense multiple access), passes the frame created in layer 2 on to layer 1 and through the shared medium to destination
	- layer 2 has access control, so if device 2 needs to send data back to device 1, it will wait until device 1 is done 
	- encapsulation is taking the data being transmitted and wrapping it in the frame
- CD (collision detection) if collisions are detected, both backoff for a time + random. increased if another close collision
- hubs are layer 1 devices and dont understand frames, all devices will receive the frame but see they aren't the destination and discard it
- if laptops connected to the hub transmitted frames at the same time it would cause a collision

- switch is a layer 2 device, works like a hub but understands layer 2
- switches have a MAC address table and store the MAC address and ether port of connected devices
- since it has a MAC address table, it'll forward the frame using the destination devices port rather than MAC address
	- wont forward collisions, since each port is a collision domain
- unicast communications one to one
- broadcast communications one to all

LAYER3:
- Network, requires 1 or more layer 2 networks to function. It's job is to get data from one location to another
- LAN1 and LAN2 are isolated local area networks, if we used only layer 2, they could only communicate with a direct point to point link using the SAME layer 2 protocol
- Routers are a layer 3 device, router removed frame encapsulation and adds new frame encapsulation at every hop
- IP is a layer 3 protocol

Layer 3 IP Packet Structure-
- Every IP packet has a source and destination IP address
- layer 4 protocol is stored in IP packet (ICMP, TCP, UDP) tcp = 6, icmp = 1, udp = 17
- most of the packet is the data itself from layer 4 protocol
- TTL is in the packet structure, used to stop packets from looping forever, defines maximum number of hops before being discarded

- Subnet Mask is on the layer 3 along with IP address
	- subnet mask allows the host to determine if IP address it needs to talk with is local or remote, which influences if it needs to use default gateway
- /16 subnet mask means the first two octets are apart of the network address, and the last two octets are for the host
- 255.255.0.0 is a /16 subnet mask
- /16 is 16 1's in binary. 11111111.11111111.00000000.00000000 is 255.255.0.0 in binary
- /24 is 24 1's in binary, 11111111.11111111.11111111.00000000 is 255.255.255.0
- 133.33.0.0 is the net start, 133.33.255.255 is the net end 
- 0.0.0.0/0 is a default route and routes all packets because everything matches.
- destination 52.217.13.37, route table destination of 52.217.13.0/24, route table will select this destination as the next hop as it's the closest match
- BGP (boarder gateway protocol) allow routers to communicate with eachother to exchange which networks they know about, and this is how the internet works
- Layer 3 IP packet gets routed to 52.217.13.1/24, and arrives in layer 2 as a frame 
- ARP (address resolution protocol) 
	- process that runs between layer 2 and layer 3, ARP discovers which MAC address relates to a given IP
Layer 3 issues-
- each packet is a serparate and isolated packet, and are routed independently. Arrival of packets can be received out of order of when they were sent 
- IP provides no method to ensure ordering of packet arrival
- no communication channel, so no way to distenguish between two different applications 
- no flow control, packet loss/ddos

LAYER4:
- TCP, transmission control protocol. UDP, user datagram protocol. Both can run ontop of IP, and have different features.
- TCP is reliable and has order correction used for HTTP, SSH, etc but is slower
	- connection orriented protocol which means you set up a connection between two devices that created a bidirectional connection
- UDP is faster because it doesn't have TCP overhead like order correction. Faster but less reliable

TCP Segments-
- Segments are specific to TCP, and are inside IP packets
	- TCP Segments have source and destination ports. They dont have src and dst IP's because those are in the IP packets
TCP HEADER:
- Source port and destination port are what allow the two devices to have an open communication between eachother
- Sequence Number, used to ensure when IP packets are received, and TCP segments are pulled it, they can be correctly ordered by sequence number
- Acknowledgment is how one side can indicate that its received a certain sequence number, every segment transmitted needs to be acknowledged
	- if device is transmitting 1,2,3, and 4, received device needs to acknowledge segments 1, 2, 3 and 4.
- Flags are used to close a connection or sync
- Window defines the number of bites that you are willing to receive between ack's
	- lets the receiver allow the control of the flow received
- Checksum used for error checking, tcp layer is able to direct error and allow for re-transmission of data
- Urgent Pointer is a call feature that coordinates the transfer of the actual data. Control traffic always takes priority

- Ephemeral Port is the source port temporarily used by the source machine connecting to our server.
- Known Port (443) is the TCP port on a server receiving segments, and the server will use the ephermeral port as the destination to send segments back to

Stateless Firewall - sees an outbound and inbound transmission of data as two separate things
	- laptop ip to server, and server to laptop ip
	- TWO firewall rules would be required, one for outbound and one for inbound
Stateful Firewall - Sees the outbound and inbound connection between laptop and server as one connection
	- Allowing the outbound connection of your laptop to the server implicity allows an inbound response from the server

NAT (Network Address Translation)
	- created to address the shortage of IPv4 addresses
	- translates private IP addresses to public
	- also provides security benefits
	- only for IPv4, IPv6 doesn't need NAT because theres so many more addresses
- Static NAT device translates 1 specific private address to 1 fixed public address (internet gateway)
	- Router has NAT table that translates private IP to public IP
	- IP packet is created with private IP, as it gets passed to the NAT device (router), it translates the source IP of that packet from private to it's public IP
- Dynamic NAT translates 1 private address to 1st available public
	- Works like static NAT, but the public IP allocations are temporary allocations
	- This means you can run out of Public IP's if you have more private IP devices trying to connect at the same time then the amount of public IP's you have
- Port Address Translation (PAT) is many private addresses to 1 public address (NAT Gateway)
	- AWS  NATGW works off PAT
	- uses both IP address and ports to allow multiple devices to use the same public IP at the same time
	- source port is randomly assigned by the device
	- if two devices have the same source port, the NATGateway (router) translates the source port to a unique port number
	- with PAT, you can't initiate traffic to private devices because it has no entry in the NAT table, and the NAT device wont know who to translate that data to

4.294 billion IPv4 addresses
CLASS A- 0.0.0.0 to 127.255.255.255 - 128 networks, 16.777 million IP's each
	- 0. is reserved
	- class a is used for massive networks/businesses
CLASS B- 128.0.0.0 to 191.255.255.255 - 16.3k networks, 65k IP's each
	- generally allocated to regional authorities, who allocate to anyone that can justify the network size
	- first two octates are for the network, last two octates are for the hosts
CLASS C- 192.0.0.0 to 223.255.255.255 - 2.1 million networks, 256 IP's each
	-  for small businesses that don't need a class b or class a IP

Private addresses can't be routed over the internet, which is why you use NAT
	- 10.0.0.0 - 10.255.255.255 (1x Class A Network)
	- 172.16.0.0 - 172.31.255.255 (16x Class B Networks)
	- 192.168.0.0 - 192.168.255.255 (256x Class C Networks)



SSL & TLS (layer 4)
- TLS/SSL offers privacy, communications are encryptyed
- asymmetric encryption initially using public keys
- identity is verified between server and client
- reliable connection, protect against alteration
	- ciper suites are set of protocols used by TLS, client and server need to agree on cipher suite 
	- authentication, ensure the server certificate is authentic, and verify the server is who it says it is (certificate authority)
	- key exchange moves from asymmetric to symmetric keys in a secure way 
		- client generates pre-master key, encrypts it with the servers public key and sends it to server
		- server decrypts pre master key using private key, and now both client and server can use the pre-master key to generate master secret, which generates ongoing session keys





Decimal to Binary conversion for IPv4 addressing
- dotted decimal notation, 4 decimal numbers ranging from 0-255 seperated by periods
- human sees the IP address, computer sees 32 bits of binary, 4x8 bits, 4x bytes, 4x octets

Compare decimal number to binary position value, if smaller, binary number is 0, if equal or larger, binary number is 1 and subtract binary position value from the decimal number for new decimal value

(133).33.33.7
-	128	-	64	-	32	-	16	-	8	-	4	-	2	-	1	
-	1	-	0	-	0	-	0	-	0	-	1	-	0	-	1 

133 - 128 = 5 (133 > 128, 1 binary. 5 is the new decimal value)
5 - 64 
5 - 32
5 - 16
5 - 8 	decimal value of 5 is less than the binary position value, binary number is 0
5 - 4 = 1 (5 > 4, 1 binary. 1 is the new decimal value)
1 - 2 decimal value is lower, binary number is 0
1 - 1 = 0, binary number 1

133.(33).33.7

33 - 128
33 - 64
33 - 32 = 1
1 - 16
1 - 8
1 - 4
1 - 2
1 - 1

0	0	1	0	0	0	0	1 (decimal 33 in binary)

133.33.33.(7)

7 - 128
7 - 64 
7 - 32
7 - 16
7 - 8
7 - 4 = 3
3 - 2 = 1
1 - 1 = 0

0	0	0	0	0	1	1	1

133.33.33.7 = 10000101.00100001.00100001.00000111

- to do binary to decimal, you work from left to right adding the decimal value following the table

-	128	-	64	-	32	-	16	-	8	-	4	-	2	-	1	
-	1	-	0	-	0	-	0	-	0	-	1	-	0	-	1 
10000101 = 128 + 0 + 0 + 0 + 0 + 4 + 0 + 1 = 133
00100001 = 0 + 0 + 32 + 0 + 0 + 0 + 0 + 1 = 33
00100001 = 0 + 0 + 32 + 0 + 0 + 0 + 0 + 1 = 33
00000111 = 0 + 0 + 0 + 0 + 0 + 4 + 2 + 1 = 7

172.16.2.158 = 10101100.00010000.00000010.10011110

172 - 128 = 44			16 - 128				2 - 128					158 - 128 = 30
44 - 64					16 - 64					2 - 64					30 - 64
44 - 32 = 12			16 - 32					2 - 32					30 - 32
12 - 16					16 - 16 = 0				2 - 16					30 - 16 = 14
12 - 8 = 4				0 - 8					2 - 8					14 - 8 - 6
4 - 4 = 0				0 - 4					2 - 4					6 - 4 = 2
0 - 2					0 - 2					2 - 2 = 0				2-2 = 0
0 - 1					0 - 1					0 - 1					0 - 1

10101100
128+0+32+0+8+4+0+0=172



AWS FOUNDATIONS-

Public vs Private Services:
- public vs private is only referring to the network and has nothing to do with permissions (public or private network)
- Three different network zones
	- "Public Internet" Zone (internet services you connect to, email, websites etc)
	- "AWS Public" Zone (S3 buckets, they aren't on the public internet, but you use the public internet to get to them)
	- "AWS Private" Zone (Virtual Private Clouds, EC2 instances unless configured otherwise)
		- you can configure private zones to access public internet services and S3
- on-prem environments can access VPCs or Private Zones only if configured via VPN or direct connect	

AWS Global Infrastructure:

Globally Resilient- services that dont rely on a region. If a region fails, the service is still available
Region Resilient- services that can rely on Availability Zones for resiliency. If AZ-A fails, the service can still operate out of AZ-b
AZ Resilient- services that will fail if the AZ it operates from fails

Regions-
- AWS Regions is a region in the world that contains full AWS services
- Each region has geographic separation  - isolated fault domain in case of natural disaster
- Regions give you location control to tune for performance

Availability Zone-
- Every region has additional AZ's
- if a disaster is local to just one AZ, you can have built in resiliency by having your systems in multiple AZ's within your region
- you can connect multiple AZ's to eachother using Virtual Private Cloud

Edge Zones-
- AWS Edge Locations are local distribution points
	- companies like Netflix use edge locations to allow for low latency/fast efficient data transfers such as streaming video
		- Netflix may run their business out of multiple AWS Regions, but store video content in edge locations

VPC Basics:
VPC- Virtual Private Cloud inside AWS
- a VPC is within 1 account & 1 region
	- regionally resilient, operate through multiple availability zones
- by default they are private and isolated unless you decide otherwise
- two types of VPC's, default which is max of 1 per region, and custom VPC's and can have many per region

Default
- one per region, can be removed or recreated
- default VPC CIDR is always 172.31.0.0/16
- the subnet of each AZ in the region is /20 
- Internet Gateway, Security Group, and NACL is included in default VPC
- Subnets assign public IPv4 addressses

EC2 Key Facts 
- IAAS - provides virtual machines, unit of consumption is the instances
- private service by default, uses VPC Networking
- EC2 is AZ resilient. Instance fails if AZ fails
- Different instance sizes and capabilities
- on-demand billing per second or per hour, charged for the instance, as well as the storage the instance uses
- local on-host storage or elastic block storage

Instance lifecycle
- running, stopped and terminated
- running instances consume cpu, memory, storage, and Networking
- stopped instances don't consume cpu, memory, or networking but you are still billed for storage because the storage is still allocated to to you
- terminated instances occur zero charges, because it deletes the instance and the storage attached

Amazon Machine Image (AMI)
- AMI can be used to create an EC2 instance, or can be used to create an AMI
- similar to a server image to deploy virtual machines
- AMI contains attached permissions
	- public gives everyone the ability to launch an instance from that AMI
	- owner has implicit allow permissions
	- explicit is specific AWS accounts that are allowed
- Root Volume (boot volume)
- Block Device Mapping, maps volumes to device ID'S

RDP - port 3389 for Windows instances
SSH - port 22 for Linux instances

CloudWatch Basics
- Collects and manages operational data on your behalf
    - Metrics for AWS products, apps, on-premises
    - CloudWatch Logs for AWS products, apps, on prem
    - CloudWatch Events for AWS Services and schedules

Shared Responsibility model
- AWS is responsible for the security OF the cloud
    - Hardware/AWS global infrastructure
    - Regions, AZ, edge locations
    - Compute, storage, database, networking hardware
    - Software provided by AWS 
- The customer is responsible for the security IN the cloud
    - client-side data encryption, integrity and authentication
    - server-side encryption (file system and/or data)
    - Networking traffic protection (encryption, identity)
    - platform, application, operating system, network and firewall config
    - customer data


