# Recording s3 admin actions in QLDB - event driven design

[![N|Solid](https://d1.awsstatic.com/r2018/h/99Product-Page-Diagram_AWS-Quantum.f03953678ba33a2d1b12aee6ee530e45507e7ac9.png)](https://aws.amazon.com/qldb/)

**WARNING**: This guide is for demonstration purposes only and should only be used in a development or test AWS environment. Elevated IAM privileges are used.

### Region Selection

This demo can be deployed in any AWS region that supports the following services:

- Amazon QLDB
- AWS Lambda
- AWS SQS
- AWS SNS
- AWS EventBridge
- AWS CloudFormation
- AWS Kinesis

You can refer to the [region table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) in the AWS documentation to see which regions have the supported services.


## Implementation Instructions

Each of the following sections provides an implementation overview and detailed, step-by-step instructions.

### 1. Launch CloudFormation template.

Launch one of these AWS CloudFormation templates in the Region of your choice.

Region| Launch
------|-----
US East (N. Virginia) | [![Launch Module 1 in us-east-1](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fqldb-streaming-lab-us-east-1.s3.us-east-1.amazonaws.com%2Fdev%2Fcfn_templates%2Fs3-event-driven.yaml&stackName=s3-event-driven&param_KinesisStreamName=EventStream&param_PersonalCellNumber=%2B12345678910&param_QLDBIndexName=cloudtrail_user&param_QLDBLedgerName=cloudtrail-demo-1&param_QLDBTableName=cloudtrailByUser)
US East (Ohio) | [![Launch Module 1 in us-east-2](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Fqldb-streaming-lab-us-east-2.s3.us-east-2.amazonaws.com%2Fdev%2Fcfn_templates%2Fs3-event-driven.yaml&stackName=s3-event-driven&param_KinesisStreamName=EventStream&param_PersonalCellNumber=%2B12345678910&param_QLDBIndexName=cloudtrail_user&param_QLDBLedgerName=cloudtrail-demo-1&param_QLDBTableName=cloudtrailByUser)
US West (Oregon) | [![Launch Module 1 in us-west-2](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Fqldb-streaming-lab-us-west-2.s3.us-west-2.amazonaws.com%2Fdev%2Fcfn_templates%2Fs3-event-driven.yaml&stackName=s3-event-driven&param_KinesisStreamName=EventStream&param_PersonalCellNumber=%2B12345678910&param_QLDBIndexName=cloudtrail_user&param_QLDBLedgerName=cloudtrail-demo-1&param_QLDBTableName=cloudtrailByUser)
EU (Frankfurt) | [![Launch Module 1 in eu-central-1](https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fqldb-streaming-lab-eu-central-1.s3.eu-central-1.amazonaws.com%2Fdev%2Fcfn_templates%2Fs3-event-driven.yaml&stackName=s3-event-driven&param_KinesisStreamName=EventStream&param_PersonalCellNumber=%2B12345678910&param_QLDBIndexName=cloudtrail_user&param_QLDBLedgerName=cloudtrail-demo-1&param_QLDBTableName=cloudtrailByUser)

### 2. Fill out the CloudFormation parameters.


