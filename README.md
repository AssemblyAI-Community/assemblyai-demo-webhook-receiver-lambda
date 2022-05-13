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

This template also handles provisioning of a DynamoDB database that is used for storing data about users. The Flask application exposes two endpoints, `POST /webhook` and `GET /webhook/status/<transaction_id>`, which allow to receive webhook events and retrieve status of a transaction_id

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
Deploying assemblyai-demo-webhook-receiver-lambda to stage dev (us-east-1)
Using Python specified in "runtime": python3.8
Packaging Python WSGI handler...

endpoint: ANY - https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com
functions:
  api: assemblyai-demo-webhook-receiver-lambda-dev-api (7.9 MB)

```

_Note_: In current form, after deployment, your API is public and can be invoked by anyone. For production deployments, you might want to configure an authorizer. For details on how to do that, refer to [httpApi event docs](https://www.serverless.com/framework/docs/providers/aws/events/http-api/).

### Invocation

After successful deployment, you can post webhooks the corresponding endpoint:

``` 
https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/webhook
```

### Testing webhook

Now you can use our API to run a transcription

* Command:
```
curl --request POST \
  --url https://api.assemblyai.com/v2/transcript \
  --header 'authorization: YOUR_ASSEMBLYAI_API_TOKEN' \
  --header 'content-type: application/json' \
  --data '{"audio_url": "https://bit.ly/3yxKEIY", "webhook_url": "https://xxxx.execute-api.us-east-1.amazonaws.com/webhook?file_name=test"}'
  ```

* Result:
```
{"id": "obwd613f79-5743-4476-be50-XXXXXXXXXXX", "language_model": "assemblyai_default", "acoustic_model": "assemblyai_default", "language_code": "en_us", "status": "queued", "audio_url": "https://bit.ly/3yxKEIY", "text": null, "words": null, "utterances": null, "confidence": null, "audio_duration": null, "punctuate": true, "format_text": true, "dual_channel": null, "webhook_url": "https://xxxxxxxxx.execute-api.us-east-1.amazonaws.com/webhook", "webhook_status_code": null, "speed_boost": false, "auto_highlights_result": null, "auto_highlights": false, "audio_start_from": null, "audio_end_at": null, "word_boost": [], "boost_param": null, "filter_profanity": false, "redact_pii": false, "redact_pii_audio": false, "redact_pii_audio_quality": null, "redact_pii_policies": null, "redact_pii_sub": null, "speaker_labels": false, "content_safety": false, "iab_categories": false, "content_safety_labels": {}, "iab_categories_result": {}, "language_detection": false, "custom_spelling": null, "disfluencies": false, "sentiment_analysis": false, "sentiment_analysis_results": null, "auto_chapters": false, "chapters": null, "entity_detection": false, "entities": null}
```

Now wait 20 seconds or so and check the status with:
```
curl --url https://y2bda0fawa.execute-api.us-east-1.amazonaws.com/webhook/status/obwd613f79-5743-4476-be50-XXXXXXXXXXX
```