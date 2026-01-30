# Integration & Messaging

## SQS
- Queue service; decouple components.
- Standard (at-least-once) vs FIFO (exactly-once, ordered).

## SNS
- Pub/sub; push to SQS, Lambda, HTTP, email.

## EventBridge
- Event bus; schema registry; filtering.

## Step Functions
- Orchestrate workflows; retries, error handling.

## MQ
- Managed ActiveMQ/RabbitMQ.

**Hands-on**
- Create SNS topic and subscribe SQS.
- Trigger Lambda via SQS event source.

