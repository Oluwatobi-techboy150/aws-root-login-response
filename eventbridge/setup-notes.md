## EventBridge Rule: RootloginDetector

- Created an EventBridge rule named RootloginDetector on the **default event bus**.
- The rule uses a custom **event pattern** to detect AWS **root account sign-ins** based on CloudTrail logs.
- Target: **SNS topic** RootLoginAlertTopic to send an email notification.
- Assigned a new **execution role** automatically to allow EventBridge to publish to SNS.

The JSON pattern used for detecting root account logins is saved in:

[root-login-rule.json](./root-login-rule.json)
[root-login-rule.json](./root-login-rule.json)
