# CloudCustodianRules
General CloudCustodian Rules could enforce benchmarks

## Initilization Steps
#### MacOS [based on Cloudcustodian Documentation](https://cloudcustodian.io/docs/quickstart/index.html#install-cloud-custodian)
* `python3 -m venv custodian`
* `source custodian/bin/activate`
* `pip install c7n`
* Configure AWS credentials


## Rules List 

| # AWS Service      | Rule Name | Description | Actions (if Available)|
| ----------- | ----------- | ----------- | -----------|
| VPC   | [VPC Flow logs - S3 ](./Rules/VPC/vpc-flowLogs-enforceS3.yml) | Checks VPCs for flow Logs | Enforce VPC flow logs for the VPC configured with S3|
| VPC   | [VPC Flow logs - CloudWatch ](./Rules/VPC/vpc-flowLogs-enforceCW.yml) | Checks VPCs for flow Logs | Enforce VPC flow logs for the VPC with CloudWatch Group|