## Sync vs Async

When a lambda function is triggered by a sync event, it is up to the caller to deal with the error. Sync triggers are:
- Http
- Invoke Lambda via AWS SDK

When a lamnda fucntion is triggered by a async event, it will have a retry behavior. Async triggers can be:
- SNS
- SQS
- Dynamo Stream
- Kinesis Stream
