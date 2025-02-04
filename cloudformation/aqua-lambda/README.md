[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=aqua-serverless&templateURL=https://s3.amazonaws.com/aqua-security-public/6.2/aquaServerless.yaml)

# Description

This page contains instructions for deploying the Aqua serverless audit stack on an Amazon account.

These instructions are applicable to Aqua Enterprise (formerly CSP) Version 5.0 and above.

Your deployment will create these services:
 - SQS: to collect the audit events from Aqua NanoEnforcers
 - Lambda function: to handle and send to audit events to an Aqua gateway

A CloudFormation template is used to deploy the Aqua serverless audit stack. This can be done either with the AWS CloudFormation Management Console or the AWS Command Line interface, as explained below.

# Requirements

From Aqua Security: the ZIP file of the Aqua audit handler Lambda function. You should store it in S3.

# Before deployment

Upload the Aqua audit function to S3.

# Deployment method 1: CloudFormation Management Console

 1. Click the <b>Launch Stack</b> icon at the top of this page. This will take you to the <b>Create stack</b> function of the AWS CloudFormation Management Console.
 2. Ensure that your AWS region is set to where you are deploying Aqua Enterprise.
 3. Click "Next".
 4. Set or modify any of the parameters (per the explanations provided).
 5. Click "Next" to create the stack.

It will typically require up to 20 minutes for Aqua Enterprise to be deployed.
When completed, you can obtain the generated `AQUA_SQS_URL` from the outputs tab, under key name `aquaSQSUrl`.

# Deployment method 2: Command Line interface

1. Copy the following command:
```
aws --region us-east-1 cloudformation create-stack --capabilities CAPABILITY_NAMED_IAM \
    --stack-name aqua-serverless-audit --template-body file://aquaServerless.yaml \
    --parameters ParameterKey=AquaGatewayAddress,ParameterValue=x.x.x.x:8443 \
                 ParameterKey=AquaToken,ParameterValue=xxxx \
                 ParameterKey=S3Bucket,ParameterValue=xxxx \ 
                 ParameterKey=S3CodeKey,ParameterValue=xxxx \
                 ParameterKey=CommunicationMethod,ParameterValue=grpc \
```
In case the choosen communication method is SSH, following changes need to be done in the command:
* AquaGatewayAddress parameter value should include port 3622 (and not 8443)
* CommunicationMethod parameter value should be ssh


2. Run the AWS create-stack CLI command.

It will typically require up to 20 minutes for your stack to be created and deployed.
When completed, you can obtain the `AQUA_SQS_URL` from the console output, under key name `aquaSQSUrl`.

# Version upgrade

To upgrade your stack, modify the existing stack with the new configuration.
