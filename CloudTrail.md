# CloudTrail

Created By: Keishin CHOU
Last Edited: Apr 28, 2020 10:58 AM

### Overview

- AWS CloudTrail is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail. Events include actions taken in the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs.
- CloudTrail Logs are then stored in an S3 bucket or a CloudWatch Logs log group that you specify.

### CloudTrail Events

- An event in CloudTrail is the record of an activity in an AWS account. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail.
- CloudTrail events provide a history of both API and non-API account activity made through the AWS Management Console, AWS SDKs, command line tools, and other AWS services.
- There are two types of events that can be logged in CloudTrail: management events and data events. By default, trails log management events, but not data events.
    - Management Events
        - Management events provide information about management operations that are performed on resources in your AWS account. These are also known as *control plane operations*. Example management events include:
            - Configuring security (for example, IAM `AttachRolePolicy` API operations).
            - Registering devices (for example, Amazon EC2 `CreateDefaultVpc` API operations).
            - Configuring rules for routing data (for example, Amazon EC2 `CreateSubnet` API operations).
            - Setting up logging (for example, AWS CloudTrail `CreateTrail` API operations).
    - Data Events
        - Data events provide information about the resource operations performed on or in a resource. These are also known as *data plane operations*. Data events are often high-volume activities. Example data events include:
            - Amazon S3 object-level API activity (for example, `GetObject`, `DeleteObject`, and `PutObject` API operations).
            - AWS Lambda function execution activity (the `Invoke` API).