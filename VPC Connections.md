# VPC - Connections

Created By: Keishin CHOU
Last Edited: Apr 23, 2020 6:47 PM

### Site-to-Site VPN

- Virtual Private Gateway:
    - VPN concentrator on the AWS side of the VPN connection
    - VGW is created and attached to the VPC from which you want to create the Site-to-
    Site VPN connection
    - Possibility to customize the ASN

    ![VPC%20Connections/Screen_Shot_2020-04-23_at_16.11.20.png](VPC%20Connections/Screen_Shot_2020-04-23_at_16.11.20.png)

- Transit Gateway
    - A transit hub that you can use to interconnect multiple VPCs and the On-Premise network.
    - A Hub-and-Spoke model.
    - Transit Gateway is Regional. But you can peer Transit Gateway across Regions.
    - You can also share Transit Gateway with other AWS account using Resource Access Manager (RAM).
    - Use Route Table to define which VPC can talk to other VPCs.
    - Works with Direct Connect Gateway and VPN connections.
    - Supports IP Multicast.

    ![VPC%20Connections/Untitled.png](VPC%20Connections/Untitled.png)

- Customer Gateway
    - Software application or physical device on customer side of the VPN connection

### Direct Connect

- Provides a dedicated private connection from a remote network to your VPC
- Use Cases:
    - Increase bandwidth throughput - working with large data sets - lower cost
    - More consistent network experience - applications using real-time data feeds
    - Hybrid Environments (on-premise + cloud)
- Supports both IPv4 and IPv6
- Data in transit is **NOT encrypted** but is private.
- Combining Direct Connect with VPN will provide an IPsec-encrypted private connection

![VPC%20Connections/Untitled%201.png](VPC%20Connections/Untitled%201.png)

### Direct Connect Gateway

- Connect your data center to multiple VPCs in different Regions.
    - The VPCs must be in the same AWS account.
- The VPCs connected to the Direct Connect Gateway are **NOT Peering** to each other.

![VPC%20Connections/Untitled%202.png](VPC%20Connections/Untitled%202.png)

- Connection types
    - Dedicated Connections
        - 1Gbps or 10 Gbps capacity
        - Physical ethernet port dedicated to the customer
        - Request made to AWS first, then completed by AWS Direct Connect Partners
    - Hosted Connections
        - 50Mbps, 500 Mbps, to 10 Gbps
        - Connection requests are made via AWS Direct Connect Partners
        - Capacity can be added or removed on demand
        - 1, 2, 5, 10 Gbps available at select AWS Direct Connect Partners

### Egress Only Internet Gateway

- For IPv6 only. (IPv6 are all public addresses.)
- Similar function as a NAT. (NAT is for IPv4.)
- EOIG gives our IPv6 instances access to the Internet, but keeps those instances inaccessible.
- Editing the Route Tables is necessary.

### Private Link

- Connect a Service VPC with thousands of Customer VCPs (own or other account)
- In the private AWS Network.
- No VPC Peering, no Internet Gateway, no NAT, no Route Tables...
- Requires a Network Load Balancer (Service VCP side) and a Elastic Network Interface (Customer VPC side)
- NLB, ENI, and the Private Link all works in multiple AZ. ⇒ Fault tolerant.

![VPC%20Connections/Untitled%203.png](VPC%20Connections/Untitled%203.png)

### EC2-Classic & AWS ClassicLink

- **DEPRECATED!**
- Do not select these two in the exam.

### VPN CloudHub

- Allows multiple Customer Network connecting with each other through VPN CloudHub.
- A Hub-and-Spoke model.
    - For information about Hub-and-Spoke model

    ![VPC%20Connections/Untitled%204.png](VPC%20Connections/Untitled%204.png)

- CloudHub uses public Internet, but encrypted.