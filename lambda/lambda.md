#  Lambda Function: Root Login Event Listener

This phase was about extending my detection system to **react** automatically when a root account login is detected.

---

##  What I Built

I wrote a Python-based AWS Lambda function that gets triggered by the **EventBridge rule** I created earlier (`RootLoginEventRule`). This rule listens for `ConsoleLogin` events where the `userIdentity.type` is `Root`.

Once triggered, the Lambda function does the following:

1. Logs that a root login event was received.
2. Pretty prints the entire event payload for visibility.
3. Extracts key details:
   - The **ARN** of the user who logged in (root user)
   - The **source IP address**
   - The **event timestamp**
4. Logs those key details to CloudWatch.

---

##  What I Tested

After deploying the Lambda and linking it to the EventBridge rule, I tested it by logging in from another device using the root user.

Within seconds, I saw new entries appear in **CloudTrail** and then the **Lambda logs in CloudWatch**. The log stream confirmed:

- The correct function executed
- The sourceIPAddress, eventTime, and ARN were accurately printed
- The Lambda role was correctly assumed

---
Trigger Evidence from CloudTrail

This function was triggered automatically by EventBridge when a root login was detected:

![Event history](https://github.com/user-attachments/assets/61b9d374-e02b-4727-8db1-2a5b7d7919c8)

##  What I Learned

- How Lambda integrates with EventBridge behind the scenes.
- How to **grant permission** to EventBridge to invoke the Lambda function (via Source ARN).
- That **CloudTrail events** can be passed to Lambda as rich JSON payloads. So, there is no need to manually decode anything.
- How Lambda creates its own CloudWatch Log Group and Stream automatically (visible in CloudTrail as CreateLogStream).

---

## Security Practice Observed

The Lambda function runs under a dedicated role and **only has permission to log to CloudWatch**. It does not take any destructive action. This follows the **principle of least privilege**.

---
Trigger Evidence from CloudTrail
This function was triggered automatically by EventBridge when a root login was detected

##  Why I Paused Here

I planned to also:
- Send an email notification using SNS
- Archive the login event into S3 for evidence

The thing is, the detection pipeline already works well. So, I’ve decided to pause here and document everything clearly. I skipped the SNS email alert because it was already configured earlier, and I didn’t store the event in S3 since that was already covered in my previous project with GuardDuty findings.


---

## Code

Here’s the exact Python code I deployed in Lambda:

```python
import json

def lambda_handler(event, context):
    print("Root login event received.")
    print("Event details:")
    print(json.dumps(event, indent=4))
    
    try:
        username = event['detail']['userIdentity']['arn']
        source_ip = event['detail']['sourceIPAddress']
        event_time = event['detail']['eventTime']

        print(f"Root user ARN: {username}")
        print(f"Source IP address: {source_ip}")
        print(f"Login time: {event_time}")

    except KeyError as e:
        print(f"Missing expected field: {e}")
        
    return {
        'statusCode': 200,
        'body': json.dumps('Root login event processed successfully.')
    }

![Python code](https://github.com/user-attachments/assets/42563111-9660-4221-b1f0-39af8b44b198)
