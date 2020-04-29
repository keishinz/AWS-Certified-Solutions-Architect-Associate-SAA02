# IAM - Access Management

Created By: Keishin CHOU
Last Edited: Apr 29, 2020 10:30 AM

- Conditions
    - The Condition element (or Condition block) lets you specify conditions for when a policy is in effect. The Condition element is optional. In the Condition element, you build expressions in which you use condition operators (equal, less than, etc.) to match the condition keys and values in the policy against keys and values in the request context.
    - Context

    ```json
    {
        "Version": "2012-10-17",
        "Statement": {
            "Sid": "AllowRemoveMfaOnlyIfRecentMfa",
            "Effect": "Allow",
            "Action": [
                "iam:DeactivateMFADevice",
                "iam:DeleteVirtualMFADevice"
            ],
            "Resource": "arn:aws:iam::*:user/${aws:username}",
            "Condition": {
                "NumericLessThanEquals": {"aws:MultiFactorAuthAge": "3600"}
            }
        }
    }
    ```

    → Allows removing your own multi-factor authentication (MFA) device, but only if you have signed in using MFA in the last hour (3,600 seconds)

### Managed Policies vs Inline Policies

- Managed Policies
    1. AWS Managed Policies
        - A standalone policy that is created and administered by AWS.
        - Standalone policy means that the policy has its own Amazon Resource Name (ARN) that includes the policy name.
        - AWS managed policies are maintained and updated by AWS as new services and API operations are introduced.
    2. User Managed Policies
        - A standalone policy that is created and administered by the user.
    - Features
        - Reusability
            - A single managed policy can be attached to multiple principal entities (users, groups, and roles)
        - Central change management
            - When you change a managed policy, the change is applied to all principal entities that the policy is attached to.
        - Versioning and rolling back
            - When you change a customer managed policy, the changed policy doesn't overwrite the existing policy. Instead, IAM creates a new version of the managed policy. IAM stores up to five versions of your customer managed policies. You can use policy versions to revert a policy to an earlier version if you need to.
        - Delegating permissions management
            - You can allow users in your AWS account to attach and detach policies while maintaining control over the permissions defined in those policies.
        - Automatic updates for AWS managed policies
            - AWS maintains AWS managed policies and updates them when necessary (for example, to add permissions for new AWS services), without you having to make changes. The updates are automatically applied to the principal entities that you have attached the AWS managed policy to.
- Inline Policies
    - A policy that's embedded in an IAM identity (a user, group, or role). An inherent part of the identity.
    - Two principal entities (users, groups, roles) may include the same policy, but they are not sharing a single policy; each role has its own copy of the policy.

        → You can copy Inline policies, but they are not reusable.

    - Inline policies are useful if you want to maintain a strict one-to-one relationship between a policy and the identity that it's applied to. In addition, when you use the AWS Management Console to delete that identity, the policies embedded in the identity are deleted as well. That's because they are part of the principal entity.

### Policies and Permissions

- Policy types
    1. Identity-based policies
        - JSON permissions policy documents that you can attach to an identity (user, group of users, or role). These policies let you specify what that identity can do (its permissions).
    2. Resource-based policies
        - JSON policy documents that you attach to a resource
        - With resource-based policies, you can specify who has access to the resource and what actions they can perform on it.
    3. Permissions boundaries
        - A permissions boundary is an advanced feature in which you set the `maximum permissions` that an identity-based policy can grant to an IAM entity.
        - When you set a permissions boundary for an entity, the entity can perform only the actions that are allowed by both its identity-based policies and its permissions boundaries.
    4. Organizations SCPs
        - Use an AWS Organizations service control policy (SCP) to define the maximum permissions for account members of an organization or organizational unit (OU).
    5. Access control lists (ACLs)
        - Use ACLs to control which principals in other accounts can access the resource to which the ACL is attached.
        - ACLs are cross-account permissions policies that grant permissions to the specified principal. ACLs cannot grant permissions to entities within the same account.
        - ACLs are similar to resource-based policies, although they are the only policy type that does not use the JSON policy document structure.
    6. Session policies
        - Pass advanced session policies when you use the AWS CLI or AWS API to assume a role or a federated user.
        - Session policies are advanced policies that you pass in a parameter when you programmatically create a temporary session for a role or federated user.
        - Session policies limit the permissions that the role or user's identity-based policies grant to the session. Session policies limit permissions for a created session, but do not grant permissions.

### Identity-based Policies vs Resource-based Policies

![IAM%20Access%20Management/Untitled.png](IAM%20Access%20Management/Untitled.png)

- **JohnSmith** – John can perform list and read actions on `Resource X`. He is granted this permission by the identity-based policy on his user and the resource-based policy on `Resource X`.
- **CarlosSalazar** – Carlos can perform list, read, and write actions on `Resource Y`, but is denied access to `Resource Z`. The identity-based policy on Carlos allows him to perform list and read actions on `Resource Y`. The `Resource Y` resource-based policy also allows him write permissions. However, although his identity-based policy allows him access to `Resource Z`, the `Resource Z` resource-based policy denies that access. An explicit `Deny` overrides an `Allow` and his access to `Resource Z` is denied.
- **MaryMajor** – Mary can perform list, read, and write operations on `Resource X`, `Resource Y`, and `Resource Z`. Her identity-based policy allows her more actions on more resources than the resource-based policies, but none of them deny access.
- **ZhangWei** – Zhang has full access to `Resource Z`. Zhang has no identity-based policies, but the `Resource Z` resource-based policy allows him full access to the resource. Zhang can also perform list and read actions on `Resource Y`.

Identity-based policies and resource-based policies are both permissions policies and are evaluated together. For a request to which only permissions policies apply, AWS first checks all policies for a `Deny`. If one exists, then the request is denied. Then AWS checks for each `Allow`. If at least one policy statement allows the action in the request, the request is allowed. It doesn't matter whether the `Allow` is in the identity-based policy or the resource-based policy.

This logic applies only when the request is made within a single AWS account. For requests made from one account to another, the requester in Account A must have an identity-based policy that allows them to make a request to the resource in Account B. Also, the resource-based policy in Account B must allow the requester in Account A to access the resource. There must be policies in both accounts that allow the operation, otherwise the request fails.

### Permission Boundary +

- For example, assume that the IAM user named `ShirleyRodriguez` should be allowed to manage only Amazon S3, Amazon CloudWatch, and Amazon EC2. To enforce this rule, you can use the following policy to set the permissions boundary for the `ShirleyRodriguez` user:

```json
{
		"Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*",
                "cloudwatch:*",
                "ec2:*"
            ],
            "Resource": "*"
        }
    ]
}
```

- When you use a policy to set the permissions boundary for a user, it limits the user's permissions but does not provide permissions on its own. In this example, the policy sets the maximum permissions of `ShirleyRodriguez` as all operations in Amazon S3, CloudWatch, and Amazon EC2. Shirley can never perform operations in any other service, including IAM, even if she has a permissions policy that allows it. For example, you can add the following policy to the `ShirleyRodriguez` user:

```json
{
	  "Version": "2012-10-17",
	  "Statement": {
	    "Effect": "Allow",
	    "Action": "iam:CreateUser",
	    "Resource": "*"
	  }
}
```

- This policy allows creating a user in IAM. If you attach this permissions policy to the `ShirleyRodriguez` user, and Shirley tries to create a user, the operation fails. It fails because the permissions boundary does not allow the `iam:CreateUser` operation. Given these two policies, Shirley does not have permission to perform any operations in AWS. You must add a different permissions policy to allow actions in other services, such as Amazon S3. Alternatively, you could update the permissions boundary to allow her to create a user in IAM.

### Evaluating Effective Permissions with Boundaries

- Identity-based policies with boundaries
    - Identity-based policies are inline or managed policies that are attached to a user, group of users, or role. Identity-based policies grant permission to the entity, and permissions boundaries limit those permissions. The effective permissions are the intersection of both policy types. An explicit deny in either of these policies overrides the allow.

        ![IAM%20Access%20Management/Untitled%201.png](IAM%20Access%20Management/Untitled%201.png)

- Resource-based policies
    - Resource-based policies control how the specified principal can access the resource to which the policy is attached. Within an account, an implicit deny in a permissions boundary does not limit the permissions granted by a resource-based policy. Permissions boundaries reduce permissions that are granted to an entity by identity-based policies, and then resource-based policies provide additional permissions to the entity. In this case, the effective permissions are everything that is allowed by the resource-based policy and the intersection of the permissions boundary and the identity-based policy. An explicit deny in any of these policies overrides the allow.

        ![IAM%20Access%20Management/Untitled%202.png](IAM%20Access%20Management/Untitled%202.png)

- Organizations SCPs
    - SCPs are applied to an entire AWS account. They limit permissions for every request made by a principal within the account. An IAM entity (user or role) can make a request that is affected by an SCP, a permissions boundary, and an identity-based policy. In this case, the request is allowed only if all three policy types allow it. The effective permissions are the intersection of all three policy types. An explicit deny in any of these policies overrides the allow.

        ![IAM%20Access%20Management/Untitled%203.png](IAM%20Access%20Management/Untitled%203.png)

- Session policies
    - Session policies are advanced policies that you pass as a parameter when you programmatically create a temporary session for a role or federated user. The permissions for a session come from the IAM entity (user or role) used to create the session and from the session policy. The entity's identity-based policy permissions are limited by the session policy and the permissions boundary. The effective permissions for this set of policy types are the intersection of all three policy types. An explicit deny in any of these policies overrides the allow.

        ![IAM%20Access%20Management/Untitled%204.png](IAM%20Access%20Management/Untitled%204.png)

### Session Policies

- A resource-based policy can specify the ARN of the user or role as a principal. In that case, the permissions from the resource-based policy are added to the role or user's identity-based policy before the session is created. The session policy limits the total permissions granted by the resource-based policy and the identity-based policy. The resulting session's permissions are the intersection of the session policies and either the resource-based policy or the identity-based policy.

    ![IAM%20Access%20Management/Untitled%205.png](IAM%20Access%20Management/Untitled%205.png)

- A resource-based policy can specify the ARN of the session as a principal. In that case, the permissions from the resource-based policy are added after the session is created. The resource-based policy permissions are not limited by the session policy. The resulting session has all the permissions of the resource-based policy plus the intersection of the identity-based policy and the session policy.

    ![IAM%20Access%20Management/Untitled%206.png](IAM%20Access%20Management/Untitled%206.png)

- A permissions boundary can set the maximum permissions for a user or role that is used to create a session. In that case, the resulting session's permissions are the intersection of the session policy, the permissions boundary, and the identity-based policy. However, a permissions boundary does not limit permissions granted by a resource-based policy that specifies the ARN of the resulting session.

    ![IAM%20Access%20Management/Untitled%207.png](IAM%20Access%20Management/Untitled%207.png)

### Determine Whether a  Request Is Allowed or Denied Within an Account

![IAM%20Access%20Management/Untitled%208.png](IAM%20Access%20Management/Untitled%208.png)

### IAM Roles vs Resource-based Policies

- Using a role as a proxy

    ⇒ Account A give up its original permissions and take permissions of Account B.

- Attaching a resource-based policy to the resource

    ⇒ Account A does not give ip its permissions.

- Ex. A user in account A needs to scan a DynamoDB table in account A and dump it into a S3 bucket in account B.

    ![IAM%20Access%20Management/Untitled%209.png](IAM%20Access%20Management/Untitled%209.png)

    For more information:

    [How IAM Roles Differ from Resource-based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html)