<!--
title: 'Demo webhook receiver - Serverless Framework Python Flask API backed by DynamoDB on AWS Lambda'
description: 'This template demonstrates how to develop and deploy a simple Python Flask API service backed by DynamoDB running on AWS Lambda using the traditional Serverless Framework.'
layout: Doc
framework: v3
platform: AWS
language: Python
priority: 2
authorLink: 'https://github.com/assemblyai'
authorName: 'AssemblyAI, Inc.'

-->

# Demo webhook receiver - Serverless Framework Python Flask API backed by DynamoDB on AWS Lambda

This repo demonstrates how to develop and deploy a simple Python Flask API Webhook receiver service, backed by DynamoDB, running on AWS Lambda using the traditional Serverless Framework.


## Anatomy of the demo

This template also handles provisioning of a DynamoDB database that is used for storing data about users. The Flask application exposes two endpoints, `POST /webhook` and `GET /status/<transaction_id>`, which allow to receive webhook events and retrieve status of a transaction_id

## Usage

### Prerequisites

In order to package your dependencies locally with `serverless-python-requirements`, you need to have `Python3.8` installed locally. You can create and activate a dedicated virtual environment with the following command:

```bash
python3.8 -m venv ./venv
source ./venv/bin/activate
```


### Deployment

install dependencies with:

```
npm install
```

and then perform deployment with:

```
serverless deploy
```

After running deploy, you should see output similar to:

```bash
Deploying assemblyai-demo-webhook-receiver-lambda-api-project to stage dev (us-east-1)

✔ Service deployed to stack assemblyai-demo-webhook-receiver-lambda-api-project-dev (182s)

endpoint: ANY - https://xxxxxxxx.execute-api.us-east-1.amazonaws.com
functions:
  api: assemblyai-demo-webhook-receiver-lambda-api (1.5 MB)
```

_Note_: In current form, after deployment, your API is public and can be invoked by anyone. For production deployments, you might want to configure an authorizer. For details on how to do that, refer to [httpApi event docs](https://www.serverless.com/framework/docs/providers/aws/events/http-api/).

### Invocation

After successful deployment, you can post webhooks the corresponding endpoint:

``` 
https://xxxxxx.execute-api.us-east-1.amazonaws.com/dev/webhook
```


