# VPC - Features

Created By: Keishin CHOU
Last Edited: Apr 23, 2020 9:13 PM

### VPC Peering

- Connect two VPCs privately using AWS network.
- Must not overlapping CIDR.
- VPC Peering connection is **NOT transitive**.
- VPC Peering works **across region and across AWS account.**
- Steps:
    1. Create a Peering Connection.
    2. Select the requester VPC and the accepter VPC (can be from different account or in different Region.) 
    3. Accept this VPC peering connection request.
    4. Edit the routes of public Route Tables in each VPC to ensure they can communicate through the Peering Connection.

### VPC Endpoints

- VPC Endpoints allow you to connect to AWS services using the private AWS network instead of the public WWW network.
- Scale horizontally and redundant.
- Endpoint types:
    - Interface Endpoint
        - Provision an ENI as an entry point (must attach security group)
        - Powered by [PrivateLink](https://www.notion.so/VPC-Connections-c06fc6ebb6074b37b6034dc812352cbb#e298d63a75f7425da7bad8c177ea7ede).
        - Ensure that the attributes 'Enable DNS hostnames' and 'Enable DNS Support' are set to 'true' for your VPC. (To use private DNS names)
        - A Security Group need to be configured.
        - Work with most AWS services except DynamoDB and S3.
    - Gateway Endpoint
        - Provision a target and must be used in a route table
        - A new route in the (Private) Route Table will be added automatically.
        - Only work with DynamoDB and S3.
        - If you use CLI to Connect S3 or DynamoDB with Gateway Endpoint, you must **specify the Region.** Because the default Region is us-east-1 in CLI.

            → ex. aws s3 ls --region ap-northeast-1

### VPC Flow Logs

- Capture information about IP traffic going into your interfaces:
    - VPC Flow Logs
    - Helps to monitor & troubleshoot connectivity issues
    - Flow logs data can go to S3 / CloudWatch Logs

### Athena

- Query Flow Logs data saved in S3 using SQL.

### Bastion Host

- Bastion Host is a public instance, through which we can connect to the private instances.
- The public instance in the public Subnet.