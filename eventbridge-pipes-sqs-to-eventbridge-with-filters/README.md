# SQS to EventBridge Bus using EventBridge Pipes

![Pipes diagram](./screenshot.png)

This pattern demonstrates sending SQS messages directly to EventBridge bus using EventBridge Pipes with filtering.

Learn more about this pattern at Serverless Land Patterns: https://serverlessland.com/patterns/eventbridge-pipes-sqs-to-eventbridge-with-filters

Important: this application uses various AWS services and there are costs associated with these services after the Free Tier usage - please see the [AWS Pricing page](https://aws.amazon.com/pricing/) for details. You are responsible for any AWS costs incurred. No warranty is implied in this example.

## Requirements

- [Create an AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html) if you do not already have one and log in. The IAM user that you use must have sufficient permissions to make necessary AWS service calls and manage AWS resources.
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) installed and configured
- [Git Installed](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [AWS Serverless Application Model](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) (AWS SAM) installed

## Deployment Instructions

1. Create a new directory, navigate to that directory in a terminal and clone the GitHub repository:
   ```
   git clone https://github.com/aws-samples/serverless-patterns
   ```
1. Change directory to the pattern directory:
   ```
   cd serverless-patterns/eventbridge-pipes-sqs-to-eventbridge-with-filters
   ```
1. From the command line, use AWS SAM to deploy the AWS resources for the pattern as specified in the template.yml file:
   ```
   sam deploy --guided
   ```
1. During the prompts:

   - Enter a stack name
   - Enter the desired AWS Region
   - Allow SAM CLI to create IAM roles with the required permissions.

   Once you have run `sam deploy -guided` mode once and saved arguments to a configuration file (samconfig.toml), you can use `sam deploy` in future to use these defaults.

1. Note the outputs from the SAM deployment process. These contain the resource names and/or ARNs which are used for testing.

## How it works

The template will create an SQS queue, event bus and pipe. Sending messages to the SQS queue will trigger the pipe to forward the messages onto the event bus.

Filtering is used to only listen to `NEW` order messages from SQS (as an example to showcase filtering with pipes) and input transforms are used to map the SQS message into an EventBridge that can be used downstream.

Send SQS message that will trigger event on event bus

```sh
 # Send SQS message to be sent to EventBridge using the filter.
 aws sqs send-message \
 --queue-url=SQS_URL \
 --message-body '{"orderId":"125a2e1e-d420-482e-8008-5a606f4b2076", "customerId": "a48516db-66aa-4dbc-bb66-a7f058c5ec24", "type": "NEW"}'
```

Send SQS message that will will not trigger event on event bus

```sh
 # Send SQS message to be sent to EventBridge using the filter.
 aws sqs send-message \
 --queue-url=SQS_URL \
 --message-body '{"orderId":"125a2e1e-d420-482e-8008-5a606f4b2076", "customerId": "a48516db-66aa-4dbc-bb66-a7f058c5ec24", "type": "OLD"}'
```

## Delete stack

```bash
sam delete
```

---

Copyright 2022 Amazon.com, Inc. or its affiliates. All Rights Reserved.

SPDX-License-Identifier: MIT-0
