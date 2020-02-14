# Setting Up AWS Security Hub<a name="securityhub-settingup"></a>

You must have an AWS account to enable AWS Security Hub\. If you don't have an account, use the following procedure to create one\.

**To sign up for AWS**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Attaching the Required IAM Policy to the IAM Identity<a name="securityhub-enable-attach-policy"></a>

The IAM identity \(user, role, or group\) that you use to enable Security Hub must have the required permissions\.

To grant the permissions required to enable Security Hub, attach the following policy to an IAM user, group, or role\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "securityhub:*",
            "Resource": "*"    
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "iam:AWSServiceName": "securityhub.amazonaws.com"
                }
            }
        }
    ]
}
```

## Enabling Security Hub<a name="securityhub-enable"></a>

After you attach the required policy to the IAM identity, you use that identity to enable Security Hub\.

You can enable Security Hub from the console or the API\.

### Enabling Security Hub from the console<a name="securityhub-enable-console"></a>

When you enable Security Hub from the console, you are also given the option to enable the supported security standards\.

**To enable Security Hub**

1. Use the credentials of the IAM identity to sign in to the Security Hub console\.

1.  When you open the Security Hub console for the first time, choose **Get Started**\.

1. On the welcome page, **Security standards** lists the security standards that Security Hub supports\.

   To enable a standard, select its check box\.

   To disable a standard, clear its check box\.

   You can enable or disable a standard or its individual controls at any time\. For information about the security standards and how to manage them, see [Security Standards in AWS Security Hub](securityhub-standards.md)\.

1. Choose **Enable Security Hub**\.

### Enabling Security Hub Using the API<a name="securityhub-enable-api"></a>

To enable Security Hub from the API, use the [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_EnableSecurityHub.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_EnableSecurityHub.html) operation\.

When you enable Security Hub from the API, it automatically enables the CIS AWS Foundations security standard\.

To enable other standards, use the [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchEnableStandards.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchEnableStandards.html) operation\.

To disable standards, use the [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchDisableStandards.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchDisableStandards.html) operation\.

## Service\-Linked Role Assigned to Security Hub<a name="security-hub-enable-slr"></a>

When you enable Security Hub, it is assigned a service\-linked role named `AWSServiceRoleForSecurityHub`\. This service\-linked role includes the permissions and trust policy that Security Hub requires to do the following:
+ Detect and aggregate findings from Amazon GuardDuty, Amazon Inspector, and Amazon Macie
+ Configure the requisite AWS Config infrastructure to run compliance checks for the supported standards \(in this release, CIS AWS Foundations\)

To view the details of `AWSServiceRoleForSecurityHub`, on the **Enable Security Hub** page, choose **View service role permissions**\. For more information, see [Using Service\-Linked Roles for AWS Security Hub](using-service-linked-roles.md)\.

For more information about service\-linked roles, see [Using Service\-Linked Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) in the *IAM User Guide*\.

## Enabling AWS Config To Support Security Standards Checks<a name="securityhub-enable-config"></a>

When you enable Security Hub from the console, you also have the option to enable the supported security standards\. When you enable Security Hub from the API, then the CIS AWS Foundations standard is enabled automatically\.

Many of the controls for the security standards rely on AWS Config service\-level rules\.

If you have any of the security standards enabled, you must enable AWS Config in the account where you enabled Security Hub\. For a Security Hub master account, you must enable AWS Config in each of this account's Security Hub member accounts\.

Security Hub does not manage AWS Config for you\. If you already have AWS Config enabled, you can continue configuring its settings through the AWS Config console or APIs\.

If you do not have AWS Config enabled, you can enable it manually or you can use the AWS CloudFormation "Enable AWS Config" template in AWS CloudFormation StackSets Sample Templates\. If you use the Security Hub [multi\-account, multi\-region enablement script](https://github.com/awslabs/aws-securityhub-multiaccount-scripts), it also enables AWS Config for you\.

**Important**  
When you turn on the AWS Config recorder, choose to record all resources supported in a given Region, including global resources\.

For more information, see [Getting Started with AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/getting-started.html) in the *AWS Config Developer Guide*\.