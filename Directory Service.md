# Directory Service

Created By: Keishin CHOU
Last Edited: Apr 28, 2020 4:18 PM

### Overview

- AWS Directory Service provides multiple ways to use Amazon Cloud Directory and Microsoft Active Directory (AD) with other AWS services. Directories store information about users, groups, and devices, and administrators use them to manage access to information and resources. AWS Directory Service provides multiple directory choices for customers who want to use existing Microsoft AD or Lightweight Directory Access Protocol (LDAP)–aware applications in the cloud. It also offers those same choices to developers who need a directory to manage users, groups, devices, and access.

### AWS Managed Microsoft AD

- With AWS Managed Microsoft AD, you can easily enable your Active Directory-aware workloads and AWS resources to use managed actual Microsoft Active Directory in the AWS Cloud. Workload examples include Amazon EC2, Amazon RDS for SQL Server, custom .NET applications, and AWS Enterprise IT applications such as Amazon WorkSpaces.

### AD Connector

- AD Connector is a proxy for redirecting directory requests to your existing Microsoft Active Directory without caching any information in the cloud. AD Connector comes in two sizes, small and large. A small AD Connector is designed for smaller organizations of up to 500 users. A large AD Connector can support larger organizations of up to 5,000 users.

### Simple AD

- Simple AD is a standalone managed directory that is powered by a Linux-Samba Active Directory–compatible server.