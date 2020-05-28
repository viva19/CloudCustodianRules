# CloudCustodianRules
General CloudCustodian Rules could enforce benchmarks

## Initilization Steps
#### MacOS [based on Cloudcustodian Documentation](https://cloudcustodian.io/docs/quickstart/index.html#install-cloud-custodian)
* `python3 -m venv custodian`
* `source custodian/bin/activate`
* `pip install c7n`
* Configure AWS credentials

### Lambda role setup required for deploying Lambda functions through Custodian
* Create a role with attaced Policy with the following permissions : 
    ```{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CustodianLambdaPermissions",
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DeleteNetworkInterface",
                "ec2:CreateNetworkInterface",
                "events:PutRule",
                "events:PutTargets",
                "iam:PassRole",
                "lambda:CreateFunction",
                "lambda:TagResource",
                "lambda:CreateEventSourceMapping",
                "lambda:UntagResource",
                "lambda:PutFunctionConcurrency",
                "lambda:DeleteFunction",
                "lambda:UpdateEventSourceMapping",
                "lambda:InvokeFunction",
                "lambda:UpdateFunctionConfiguration",
                "lambda:UpdateAlias",
                "lambda:UpdateFunctionCode",
                "lambda:AddPermission",
                "lambda:DeleteAlias",
                "lambda:DeleteFunctionConcurrency",
                "lambda:DeleteEventSourceMapping",
                "lambda:RemovePermission",
                "lambda:CreateAlias",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup"
            ],
            "Resource": "*"
        }
    ]
    }
 * Name the Role as preferred . If you name it `custodian` , you would not need to replace the role names
## Rules List ( Full)

| # AWS Service      | Rule Name | Description | Actions (if Available)|
| ----------- | ----------- | ----------- | -----------|
| VPC   | [VPC Flow logs - S3 ](./Rules/VPC/vpc-flowLogs-enforceS3.yml) | Checks VPCs for flow Logs | Enforce VPC flow logs for the VPC configured with S3|
| VPC   | [VPC Flow logs - CloudWatch ](./Rules/VPC/vpc-flowLogs-enforceCW.yml) | Checks VPCs for flow Logs | Enforce VPC flow logs for the VPC with CloudWatch Group|
| VPC   | [ VPC endpoint checks for S3 and KMS ](./Rules/VPC/vpc-endpoints.yml) | If organization is using VPC endpoints, this checks for Endpoints bieng used for S3 and KMS without PrivateDNS Enabled| N/A|


## Rules List ( ISO Objective : Organization of information security )

|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| VPC | [VPC Flow Logs S3](./Rules/VPC/vpc-flowLogs-enforceS3.yml) / [VPC Flow Logs CloudWatch](./Rules/VPC/vpc-flowLogs-enforceCW.yml) | Rule checks for VPC to enable Flow logs and automate the enforcement
| IAM | [ Disable IAM keys older than 90 Days](./Rules/IAM/IAM-disable-keys-older-than-90days.yml)| Checks for IAM keys older than 90 days and disable the keys
| IAM | [ full permissions policy bieng used](./Rules/IAM/IAM-full-permissions-used-policy.yml)| Checks for IAM policies for full policy bieng used
| EC2| [Terminate public instance when launched](./Rules/EC2/ec2-terminate-instances-with-publicIP.yml) | Checks for EC2 instances attached with Public IP address via CLoudwatch events.
| RDS| [ Terminate Public and Encrypted RDS instance](./Rules/RDS/RDS-terminate-public-unecrypted-instance.yml) | Terminate RDS instances with Public IP addresses and Uncrypted via the CloudWatch Events. 

## Rules List ( ISO Objective : Asset management)
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| RDS | [Mandatory EC2 Tags](./Rules/RDS/RDS-mandatory-tagging.yml) | Checks for mandatory tags ie: classification,costcenter,project |
| EC2 | [Mandatory RDS Tags ](./Rules/EC2/ec2-mandatory-tags.yml) | Checks for mandatory tags ie: classification, costcenter,project |
| RDS| [Bechmark checks for confidential RDS instance](./Rules/RDS/RDS-bechmarks-confidential.yml) | Checks for Confidential classified RDS instances not implementing the required practices |
| EC2 | [Bechmark checks for Confidential EC2 instance](./Rules/EC2/ec2-check-benchmarks-confidential.yml) | Checks for Confidential classified EC2 instance with best practices |
| EC2 | [Bechmark checks for Public EC2 instance](./Rules/EC2/ec2-check-benchmarks-public.yml) | Checks for public EC2 instances|
| S3 | [Benchmarks for Log S3 Buckets](./Rules/S3/s3-best-practices-logbuckets.yml) | Checks for logs buckets with best practices |
| S3 | [Benchamarks for Confidential S3 Buckets](./Rules/S3/s3-best-practices-confidential.yml) | Checks for confidential buckets with best practices | 
| S3 | [Benchmarks for General S3 Buckets](./Rules/S3/s3-best-practices-general.yml) | Checks for general S3 buckets with best practices |






Sectoion 9 : Access Control 
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| IAM | [disabled keys older than 90 days](./Rules/IAM/IAM-disable-keys-older-than-90days.yml) | Rules automatically disables IAM Access keys older than 90 days| 
| IAM | [Checks for policy used with full AWS access](./Rule/IAM/../../Rules/IAM/IAM-full-permissions-used-policy.yml) | Identifies used policies with full AWS access |
| IAM | [Checks for IAM users with privileged access to permissions management](./Rule/IAM/../../Rules/IAM/IAM-users-with-privilegedAccess.yml) | Identifies Users with access to make changes to permisisons |
| IAM | [Checks for security port allowing SSH, RDP ports](./Rules/SecurityGroup/security-group-allow-public-ssh-rdp.yml) | Identifies security groups which allow 0.0.0.0 access to SSH and RDP |
|S3 | [Checks for S3 buckets against our best practices](./Rules/S3/s3-best-practices-confidential.yml)| Indentifies the S3 buckets violating our best practices | 
| KMS| [Checks for KMS with cross region and high Grant Count](./Rules/KMS/kms-best-practices.yml) | Indentifies KMS keys violating best practice . Sets Key rotation if disabled
| IAM | [Checks for Users with Privileged EC2 Access](./Rules/IAM/IAM-users-with-privilegedEC2Access.yml)| Identifies Users with access to EC2 privileges |
| IAM | [Checks for Users with Privileged RDS Access](./Rules/IAM/IAM-users-with-privilegedRDSAccess.yml)| Identifies Users with access to RDS privileges|


Cryptography:
KMS 
ACM
ALB
ELB
RDS for encryption
EC2 for encryption

