# assignment2.15chichao

# AWS Secrets Manager Access from EC2

This document answers the questions related to authorizing an EC2 instance to retrieve secrets from AWS Secrets Manager. Below are the detailed answers:

## Question 1: What is needed to authorize your EC2 to retrieve secrets from the AWS Secrets Manager?

To authorize an EC2 instance to retrieve secrets from AWS Secrets Manager, the following steps are required:

1. **IAM Role:** Attach an IAM role to the EC2 instance.
   - The IAM role should have the necessary permissions to access the secret stored in AWS Secrets Manager.

2. **IAM Policy:** Attach a policy to the IAM role that specifies the permissions for accessing the secret.

3. **Secret ARN:** Use the specific Amazon Resource Name (ARN) of the secret in the policy to restrict access.

4. **AWS SDK/CLI Configuration:** Ensure the AWS SDK or CLI is configured on the EC2 instance to use the IAM role's credentials for authentication.

## Question 2: Derive the IAM policy (i.e. JSON)?

Below is the JSON for an IAM policy that grants read-only access to a specific secret in AWS Secrets Manager:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue",
        "secretsmanager:DescribeSecret"
      ],
      "Resource": "arn:aws:secretsmanager:<region>:<account-id>:secret:prod/cart-service/credentials"
    }
  ]
}
```

### Key Points:
- **Actions:**
  - `secretsmanager:GetSecretValue`: Allows retrieval of the secret's value.
  - `secretsmanager:DescribeSecret`: Allows viewing details about the secret.
- **Resource:** Specifies the ARN of the secret. Replace `<region>` and `<account-id>` with your AWS region and account ID, respectively.

## Question 3: Using the secret name `prod/cart-service/credentials`, derive a sensible ARN as the specific resource for access

The ARN for the secret `prod/cart-service/credentials` would follow this format:

```
arn:aws:secretsmanager:<region>:<account-id>:secret:prod/cart-service/credentials
```

### Example ARN:
If your AWS region is `us-east-1` and your account ID is `123456789012`, the ARN would be:

```
arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/cart-service/credentials
```

## Additional Notes
- Ensure that the EC2 instance has the IAM role attached with the above policy.
- Use the AWS SDK (e.g., `boto3` in Python) or AWS CLI to retrieve the secret using the `GetSecretValue` API.
- Restrict permissions to the minimum necessary resources and actions to follow the principle of least privilege.
