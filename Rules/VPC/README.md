## VPC Rules

### Variables Required:

In order to enforce automation for VPC flow logs , we would need to setup one of the following :

* #### Create a S3 bucket
  * Once you have created a S3 bucket for VPC flowLogs, Copy to ARN 
  * Copy the ARN to VPC-flowLogs-enforceS3.yml to LogDestination field. (Example: arn:aws:s3:::test-bucket)
* #### Create Role for CloudWatch Logs 
  * Create a role with the following permissions:
    ``` {
    "Statement": [
        {
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
    }
  * Create Trust relationship to the role : 
    ```{
    "Version": "2012-10-17",
    "Statement": [
        {
        "Sid": "",
        "Effect": "Allow",
        "Principal": {
            "Service": "vpc-flow-logs.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
        }
    ]
    }
  * Copy the ARN of the Role to the DeliverLogsPermissionArn field. Example ( arn:aws:iam::{account_id}:role/flowlogsRole)
  * Optional : Change the Log group as needed for the LogGroupName field. 
