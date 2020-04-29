# VPC - Fundamental

Created By: Keishin CHOU
Last Edited: Apr 23, 2020 9:11 AM

### Basic Concepts

- CIDR-IPv4
    - Base IP address: 10.0.0.0
    - Subnet mask: x.x.x.x/(0-32)
        - /32 allows for 1 IP = 2^(32-32)
        - /31 allows for 2 IP = 2^(32-31)
        - /28 allows for 16 IP = 2^(32-28)
        - /32 no IP can change
        - /24 last IP number can change
        - /16 last IP two number can change
        - /8 last IP three number can change
        - /0 all IP numbers can change
- Private IP
    - 10.0.0.0/8 ⇒ 10.0.0.0 ~ 10.255.255.255
    - 172.16.0.0/12 ⇒ 172.16.0.0 ~ 172.31.255.255 : Default AWS one
    - 192.168.0.0/16 ⇒ 192.168.0.0 ~ 192.168.255.255
- Public IP
    - All the rest IPs.

### VPC Overview

- Virtual Private Cloud
- Region bounded. max 5 VPCs per region - soft limit.
- 5 CIDRs per VPC at most. For each CIDR:
    - Min size is /28 = 16 IP addresses
    - Max size is /16 = 65535 IP addresses
- VPC **CIDR should not OVERLAP** with other networks. CIDR in each subnet should never be overlapped.

### Subnet - IPv4

- AWS reserves 5 IP addresses in each subnet which cannot be used or assigned to an instance.
    - 10.0.0.0: Network address
    - 10.0.0.1: Reser ved by AWS for the VPC router
    - 10.0.0.2: Reserved by AWS for mapping to Amazon-provided DNS
    - 10.0.0.3: Reserved by AWS for future use
    - 10.0.0.255:Network broadcast address.AWS does not support broadcast in aVPC,
    therefore the address is reserved
- If you need 29 IP addresses for EC2 instances, you can’t choose a Subnet of size /27 (32 IP). You need at least 64 IP,Subnet size /26 (64-5 = 59 > 29,but 32-5 = 27 < 29)

### Internet Gateway

- Internet gateways helps our VPC instances connecting the internet.
- One VPC can only be attached to one IGW and vice versa.
- Internet Gateways itself will not allow Internet accessing by default.
- To access the Internet, Route Table must be attach to public instances and configured.

### Route Table

- Default Route Table will be created when you create a VPC.
- Default Route Table only allow private network accessing.
- To access the public Internet, you must create another Route Table(for Public Instances) and edit the Route to open to the Internet through the Internet Gateway.
    - Destination = 0.0.0.0/0
    - Target = Internet Gateway attached to the VPC.
- Set up the Subnet Association in each Route Table.
    - Public Subnets → Public Route Table
    - Private Subnets → Private Route Table

### NAT Instance

- NAT: Network Address Translation
- NAT Instance: an EC2 instance configured for NAT usage. Search in Community AMIs.
    - Allows instances in the private subnets to connect to the internet
    - Must be launched in a public subnet
    - **Must disable EC2 flag: Source / Destination Check**
    - Must have an Elastic IP attached to it
    - Route table must be configured to route traffic from private subnets to
    NAT Instance

### NAT Gateway

- AWS managed NAT, higher bandwidth, better availability, no admin.
- **NAT Gateway is resilient within a single-AZ**.
    - You must create multiple NAT Gateway in multiple AZ for fault tolerance.
- You cannot associate a Security Group with NAT Gateway.

### NAT Instance vs NAT Gateway

[Comparison of NAT Instances and NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html)

[NAT Instance vs. NAT Gateway - DZone Cloud](https://dzone.com/articles/nat-instance-vs-nat-gateway)

### Network ACLs

- ACLs are firewall at Subnet level.
- Default ACL allows everything outbound and everything inbound.
- User created ACLs by default denies all inbound and outbound traffic until you add rules.
- One ACL per Subnet. New created Subnets are assigned to the default Network ACL.
- One ACL can be associated with multiple Subnets.
- ACL rules:
    - Each rule can either allow or deny traffic.
    - Rules have a number (1-32766) and higher precedence with a lower number. 100 > 200
    - Last rule is an asterisk (*) and denies a request in case of no rule match.
- ACL is a good way to block a specific IP at Subnet level.

[Network ACLs vs Security Groups](VPC%20Fundamental/Network%20ACLs%20vs%20Security%20Groups.csv)

- Stateful vs Stateless [reference]

[yohei-y:weblog](http://yohei-y.blogspot.com/2007/10/blog-post.html)