# IAM

Created By: Keishin CHOU
Last Edited: Apr 29, 2020 8:43 AM

### Overview

- AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources.
- Control who is authenticated (signed in) and authorized (has permissions) to use resources.

### Emements

- Principal
    - A principal is a `person` or `application` that can make a request for an action or operation on an AWS resource.
- Request
    - When a principal tries to use the AWS Management Console, the AWS API, or the AWS CLI, that principal sends a *request* to AWS. The request includes the following information:
        - **Actions or operations** – The actions or operations that the principal wants to perform. This can be an action in the AWS Management Console, or an operation in the AWS CLI or AWS API.
        - **Resources** – The AWS resource object upon which the actions or operations are performed.
        - **Principal** – The person or application that used an entity (user or role) to send the request. Information about the principal includes the policies that are associated with the entity that the principal used to sign in.
        - **Environment data** – Information about the IP address, user agent, SSL enabled status, or the time of day.
        - **Resource data** – Data related to the resource that is being requested. This can include information such as a DynamoDB table name or a tag on an Amazon EC2 instance.