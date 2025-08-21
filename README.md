# AWS IAM User & Role CloudFormation Template

## Overview
This CloudFormation template provisions an **IAM User** and optionally an **IAM Role**, with support for attaching either **AWS Managed Policies** or **Inline Policies (as JSON strings)**. Resources are tagged and follows structured naming conventions. Optionally creates Access Key.

## Features
- **IAM User Creation**: Generates a named IAM user with either a managed or inline policy.
- **IAM Role Management**: Optionally creates an IAM role or uses an existing one.
- **Flexible Policy Attachment**:
  - Use AWS Managed Policies (by name).
  - Use custom Inline Policies (as JSON).
- **AssumeRole Permission**: Grants the IAM user permission to assume the role.
- **Metadata Tagging**: Adds structured metadata for ownership, environment, and purpose.

## Usage Instructions

1. **Choose IAM Role Behavior**:
   - Set `CreateNewRole` to `"true"` to create a new IAM role.
   - If using an existing IAM role, set `CreateNewRole` to `"false"` and provide its ARN via `ExistingRoleArn`.

2. **Configure IAM User Policy**:
   - Set `IAMUserPolicyType` to `"Managed"` and specify the policy name via `IAMUserPolicy`, _or_
   - Set `IAMUserPolicyType` to `"Inline"` and provide a policy document string via `IAMUserInlinePolicy`.

3. **Configure IAM Role Policy**:
   - Set `IAMRolePolicyType` to `"Managed"` and specify the policy name via `IAMRolePolicy`, _or_
   - Set `IAMRolePolicyType` to `"Inline"` and provide a policy document string via `IAMRoleInlinePolicy`.

4. **Set Resource Naming Parameters**:
   - Use `OrganizationName`, `Purpose`, `Purpose2`, `Region`, `Environment`, and `ResourceNumber` for naming the resource.
5. **Deploy the Stack**:
   - Use the AWS CLI or Console to deploy the template, passing in the required parameters.

## Example Inline Policy JSON

`
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    }
  ]
}
`


| Parameter                    | Description                                                                                                             | Example Value                                        | Required                                          |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------- |
| **CreateNewRole**            | Whether to create a new IAM role. Set to `"true"` to create, `"false"` to use an existing role.                         | `"true"`                                             | Mandatory                                         |
| **ExistingRoleArn**          | ARN of an existing IAM role (used only if `CreateNewRole` is `"false"`).                                                | `"arn:aws:iam::accountnumber:role/ExistingRoleName"` | Conditional (if `CreateNewRole` is `false`)       |
| **IAMUserPolicyType**        | Type of policy to attach to the IAM User. Valid values: `"Managed"` or `"Inline"`.                                      | `"Managed"`                                          | Mandatory                                         |
| **IAMUserPolicy**            | Name of the AWS Managed Policy to attach to the IAM User. Required if `IAMUserPolicyType` is `"Managed"`.               | `"IAMReadOnlyAccess"`                                | Conditional (if `IAMUserPolicyType` is `Managed`) |
| **UserInlinePolicyName**     | Name of the inline policy for the IAM User. Used only when `IAMUserPolicyType` is `"Inline"`.                           | `"CFL-Pol-DevOps-ReadOnly-Lon-01"`                   | Conditional (if `IAMUserPolicyType` is `Inline`)  |
| **IAMUserInlinePolicy**      | JSON-formatted string representing the inline policy for the IAM User. Used only when `IAMUserPolicyType` is `"Inline"` | `{"Version": "2012-10-17", "Statement": [...]}`      | Conditional (if `IAMUserPolicyType` is `Inline`)  |
| **IAMRolePolicyType**        | Type of policy to attach to the IAM Role. Valid values: `"Managed"` or `"Inline"`.                                      | `"Managed"`                                          | Mandatory                                         |
| **IAMRolePolicy**            | Name of the AWS Managed Policy to attach to the IAM Role. Required if `IAMRolePolicyType` is `"Managed"`.               | `"IAMReadOnlyAccess"`                                | Conditional (if `IAMRolePolicyType` is `Managed`) |
| **RoleInlinePolicyName**     | Name of the inline policy for the IAM Role. Used only when `IAMRolePolicyType` is `"Inline"`.                           | `"CFL-Pol-DevOps-RoleAccess-Lon-01"`                 | Conditional (if `IAMRolePolicyType` is `Inline`)  |
| **IAMRoleInlinePolicy**      | JSON-formatted string representing the inline policy for the IAM Role. Used only when `IAMRolePolicyType` is `"Inline"` | `{"Version": "2012-10-17", "Statement": [...]}`      | Conditional (if `IAMRolePolicyType` is `Inline`)  |
| **UserAssumeRolePolicyName** | Name of the policy allowing the IAM User to assume the IAM Role.                                                        | `"CFL-Role-DevOps-Allow-AssumeRole"`                 | Mandatory                                         |
| **CreateAccessKey**          | If `"true"`, creates an access key for the IAM User (programmatic access).                                              | `"false"`                                            | Mandatory                                          |
| **InitialPassword**          | Initial console login password for the IAM User. Must meet AWS password policy.                                         | `"Temp#Pass123"`                                     | Mandatory                                          |
| **OrganizationName**         | Name of your organization (used in naming resources).                                                                   | `"CFL"`                                              | Mandatory                                         |
| **Purpose1**                 | Primary purpose of this resource, used in naming. Must start with a capital letter.                                     | `"DevOps"`                                           | Mandatory                                         |
| **Purpose2**                 | Optional secondary purpose for the resource.                                                                            | `"Create"`                                           | Optional                                          |
| **ResourceNumber**           | Unique number used for resource naming.                                                                                 | `"01"`                                               | Mandatory                                         |
| **CreatedBy**                | Name of person who created resource (for tagging).                                                                  | `"cloudformation"`                                   | Mandatory                                         |
| **Owner**                    | Owner for the resource. (for tagging).                                                                            | `"DevOps"`                                           | Mandatory                                         |
| **OwnerEmail**               | Email address of the resource owner.   (for tagging).                                                                                 | `"CFL-DevOps@communityfibre.co.uk"`                  | Mandatory                                         |
| **ProjectName**              | Project name Must start with a capital letter. (for tagging and naming)                                                 | `"Hobs"`                                             | Mandatory                                         |
| **Purpose**                  | Purpose for creating resource (for tagging).                                                           | `"Automation"`                                       | Mandatory                                         |
| **Region**                   | AWS region code. Must start with a capital letter.(for tagging)                                                                      | `"Lon"`                                              | Mandatory                                         |
| **AppName**                  | Name of the application the resource belongs to(for tagging).                                                                        | `"Hobs OSS BSS"`                                     | Mandatory                                         |
| **Environment**              | Environment for deployment. Choose from: Dev, Prod, Uat, Sit, etc.                                                      | `"Dev"`                                              | Mandatory                                         |
| **Ticket**                   | Ticket number associated with this deployment.(for tagging).                                                             | `"DIS-123"`                                          | Mandatory                                         |
| **AppEnv**                   | Application environment string, usually combination of app and env.(for tagging).                                                     | `"Hobs_Dev"`                                         | Mandatory                                         |
| **AppOwner**                 | Name of the application owner(for tagging).                                                                                         | `"Devops"`                                           | Mandatory                                         |
| **AppOwnerEmail**            | Email of the application owner.(for tagging).                                                                                         | `"Devops@communityfibre.co.uk"`                      | Mandatory                                         |
