## Policy Notes

1. eventbridge_trust_policy.json – This trust policy allows EventBridge to assume the IAM role in order to invoke Lambda.

2. lambda_execution_policy.json – Grants Lambda permission to write logs to CloudWatch. It also includes permission to publish messages to SNS, if you decide to enable SNS alerts later.

   _Note: Replace YOUR_SNS_TOPIC_ARN with your actual SNS topic ARN when setting it up._

3. sns_publish_policy.json – Use this as a dedicated policy if you want to isolate SNS publishing permissions. Otherwise, you can merge these permissions into the Lambda execution policy above.

I intentionally did not include my AWS Account ID, SNS Topic ARN, or other sensitive values in these policy files for security reasons.

These .json files are meant to serve as templates only. You should replace the placeholders with your actual values when deploying the project.

