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
 * Name the Role as preferred. If you name it `custodian` , you would not need to replace the role names
## Rules List ( Full)

| # AWS Service      | Rule Name | Description | Actions (if Available)|
| ----------- | ----------- | ----------- | -----------|
| VPC   | [VPC Flow logs - S3 ](./Rules/VPC/vpc-flowLogs-enforceS3.yml) | Checks VPCs for flow Logs | Enforce VPC flow logs for the VPC configured with S3|
| VPC   | [VPC Flow logs - CloudWatch ](./Rules/VPC/vpc-flowLogs-enforceCW.yml) | Checks VPCs for flow Logs | Enforce VPC flow logs for the VPC with CloudWatch Group|
| VPC   | [ VPC endpoint checks for S3 and KMS ](./Rules/VPC/vpc-endpoints.yml) | If organization is using VPC endpoints, this checks for Endpoints bieng used for S3 and KMS without PrivateDNS Enabled| N/A|


## Rules List ( ISO Objective: Organization of information security )

|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| VPC | [VPC Flow Logs S3](./Rules/VPC/vpc-flowLogs-enforceS3.yml) / [VPC Flow Logs CloudWatch](./Rules/VPC/vpc-flowLogs-enforceCW.yml) | Rule checks for VPC to enable Flow logs and automate the enforcement
| IAM | [ Disable IAM keys older than 90 Days](./Rules/IAM/IAM-disable-keys-older-than-90days.yml)| Checks for IAM keys older than 90 days and disable the keys
| IAM | [ full permissions policy bieng used](./Rules/IAM/IAM-full-permissions-used-policy.yml)| Checks for IAM policies for full policy bieng used
| EC2| [Terminate public instance when launched](./Rules/EC2/ec2-terminate-instances-with-publicIP.yml) | Checks for EC2 instances attached with Public IP address via CLoudwatch events. |
| RDS| [ Terminate Public and Encrypted RDS instance](./Rules/RDS/RDS-terminate-public-unencrypted-instance.yml) | Terminate RDS instances with Public IP addresses and Unencrypted via the CloudWatch Events. |

## Rules List ( ISO Objective : Asset management)
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| RDS | [Mandatory RDS Tags](./Rules/RDS/RDS-mandatory-tagging.yml) | Checks for mandatory tags ie: classification,costcenter,project |
| EC2 | [Mandatory EC2 Tags ](./Rules/EC2/ec2-mandatory-tags.yml) | Checks for mandatory tags ie: classification, costcenter,project |
| RDS| [Benchmark checks for confidential RDS instance](./Rules/RDS/RDS-Benchmarks-confidential.yml) | Checks for Confidential classified RDS instances not implementing the required practices |
| EC2 | [Benchmark checks for Confidential EC2 instance](./Rules/EC2/ec2-check-benchmarks-confidential.yml) | Checks for Confidential classified EC2 instance with best practices |
| EC2 | [Benchmark checks for Public EC2 instance](./Rules/EC2/ec2-check-benchmarks-public.yml) | Checks for public EC2 instances|
| S3 | [Benchmarks for Log S3 Buckets](./Rules/S3/s3-best-practices-logbuckets.yml) | Checks for logs buckets with best practices |
| S3 | [Benchamarks for Confidential S3 Buckets](./Rules/S3/s3-best-practices-confidential.yml) | Checks for confidential buckets with best practices | 
| S3 | [Benchmarks for General S3 Buckets](./Rules/S3/s3-best-practices-general.yml) | Checks for general S3 buckets with best practices |






Section 9: Access Control 
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| IAM | [disabled keys older than 90 days](./Rules/IAM/IAM-disable-keys-older-than-90days.yml) | Rules automatically disables IAM Access keys older than 90 days| 
| IAM | [Checks for policy used with full AWS access](./Rule/IAM/../../Rules/IAM/IAM-full-permissions-used-policy.yml) | Identifies used policies with full AWS access |
| IAM | [Checks for IAM users with privileged access to permissions management](./Rule/IAM/../../Rules/IAM/IAM-users-with-privilegedAccess.yml) | Identifies Users with access to make changes to permissions |
| SG | [Checks for security port allowing SSH, RDP ports](./Rules/SecurityGroup/security-group-allow-public-ssh-rdp.yml) | Identifies security groups which allow 0.0.0.0 access to SSH and RDP |
|S3 | [Checks for S3 buckets against our best practices](./Rules/S3/s3-best-practices-confidential.yml)| Identifies the S3 buckets violating our best practices | 
| KMS| [Checks for KMS with the cross region and high Grant Count](./Rules/KMS/kms-best-practices.yml) | Identifies KMS keys violating best practice and Sets Key rotation if disabled |
| IAM | [Checks for Users with Privileged EC2 Access](./Rules/IAM/IAM-users-with-privilegedEC2Access.yml)| Identifies Users with access to EC2 privileges |
| IAM | [Checks for Users with Privileged RDS Access](./Rules/IAM/IAM-users-with-privilegedRDSAccess.yml)| Identifies Users with access to RDS privileges|


Cryptography:
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
|KMS| [Check for non-CMK keys](./Rules/KMS/kms-non-CMK-used.yml)| Rules checks for non-cmk keys used|
|KMS| [Check for ViaService Statement bieng used](Rules/KMS/kms-check-ViaService.yml) | Rules to check ViaService is being used|
| KMS | [Check for KMS best practices](./Rules/KMS/kms-best-practices.yml)| Rules to check for KMS general practices|
| ELB| [Classic Load Balanacer does not use SSL](./Rules/ELB/ELB-no-SSL-used.yml) | Rules to check for non-ssl configured ELB used|
| ALB| [Application Load Balancer configured with HTTP](./Rules/ELB/ALB-HTTP-used.yml)| Rules to check for ALB configured for HTTP|
| RDS| [RDS Non-encrypted bieng used](./Rules/RDS/RDS-Storage-not-encrypted.yml)| Rule checks for RDS without encryption |
|EC2| [EC2 instances with non-encrypted EBS Volumes](./Rules/EC2/ec2-check-non-encrypted-volumes.yml)| Rule checks for EC2 instances without non encrypted EBS volume|



Operational Security
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
|RDS| [RDS with snapshot retention less than seven days](./Rules/RDS/RDS-with-backup-less-than-7days.yml)| Checks for RDS instances with less than seven days of snapshot retention|
|RDS|[RDS without logging setup](./Rules/RDS/RDS-without-logging.yml)| Checks for RDS instances with no logging setup|
|EC2|[EC2 instances without monitoing used](./Rules/EC2/ec2-monitoring-disabled.yml)| Checks for all the EC2 instances with monitoring state disabled|
|EBS|[EBS volumes which are not fault tolerant](./Rules/EC2/ebs-not-fault-tolerant.yml)| Checks for EBS volumes which do not have snapshots taken for last 7 days|
|S3|[S3 Buckets configured without logging]| Checks for S3 buckets without logging setup|
|ALB| [ALBs without logging ](./Rules/ELB/ALB-logging-not-enabled.yml)| Checks for ALBs without logging|
|ELB|[ELB without logging](./Rules/ELB/ELB-logging-not-enabled.yml)|Checks for Classic ELB configured without logging|
|Cloudtrail|[Cloud Trails violating best practices](./Rules/Cloudtrail/Cloudtrail-best-practices.yml)|Checks for CloudTrail against best practices|

Communication Security
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| ELB| [Classic Load Balanacer does not use SSL](./Rules/ELB/ELB-no-SSL-used.yml) | Rules to check for non-ssl configured ELB used|
| ALB| [Application Load Balancer configured with HTTP](./Rules/ELB/ALB-HTTP-used.yml)| Rules to check for ALB configured for HTTP|
| EC2 | [Benchmark checks for Public EC2 instance](./Rules/EC2/ec2-check-benchmarks-public.yml) | Checks for public EC2 instances|
|SG|[Security Groups allow HTTP ](./Rules/SecurityGroup/security-group-allow-HTTP.yml)| Checks for Security group allowing public HTTP|

##### TODO Write a rule to check for Ciphers


Section 14: System acquisition, development and maintenance 
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| EC2 | [Benchmark checks for Public EC2 instance](./Rules/EC2/ec2-check-benchmarks-public.yml) | Checks for public EC2 instances|
| S3 | [Benchmarks for General S3 Buckets](./Rules/S3/s3-best-practices-general.yml) | Checks for general S3 buckets with best practices |
| RDS| [ Terminate Public and unencrypted RDS instance](./Rules/RDS/RDS-terminate-public-unecrypted-instance.yml) | Terminate RDS instances with Public IP addresses and unencrypted via the CloudWatch Events. |
| RDS| [Benchmark checks for confidential RDS instance](./Rules/RDS/RDS-Benchmarks-confidential.yml) | Checks for Confidential classified RDS instances not implementing the required practices |
|SG|[Security Groups allow HTTP ](./Rules/SecurityGroup/security-group-allow-HTTP.yml)| Checks for Security group allowing public HTTPS|
|SG|[Security Groups allow Public SSH and RDP](./Rules/SecurityGroup/security-group-allow-public-ssh-rdp.yml)| Checks for Security group allowing public SSH and RDP|
|RDS|[RDS Snapshot shared cross-Account](./Rules/RDS/RDS-snapshot-crossaccount.yml)| Checks for RDS snapshots shared cross accounts|
|EBS|[EBS Snapshots shared cross Account](./Rules/EC2/ebs-snapshot-crossaccount.yml)|Checks for Cross acount shared EBS snapshots|


Section 17: Information security aspects of business continuity management 
|  AWS Service      | Rule Name | Description | 
| ----------- | ----------- | ----------- | 
| EC2 | [Benchmark checks for Confidential EC2 instance](./Rules/EC2/ec2-check-benchmarks-confidential.yml) | Checks for Confidential classified EC2 instance with best practices |
| EC2 | [Benchmark checks for Public EC2 instance](./Rules/EC2/ec2-check-benchmarks-public.yml) | Checks for public EC2 instances against best practices|
|RDS| [RDS with snapshot retention less than 7 days](./Rules/RDS/RDS-with-backup-less-than-7days.yml)| Checks for RDS instances with less than 7 days of snapshot retention|
|EBS|[EBS volumes which are not fault tolerant](./Rules/EC2/ebs-not-fault-tolerant.yml)| Checks for EBS volumes which do not have snapshots taken for last 7 days|
| RDS| [Benchmark checks for confidential RDS instance](./Rules/RDS/RDS-Benchmarks-confidential.yml) | Checks for Confidential classified RDS instances not implementing the required practices |