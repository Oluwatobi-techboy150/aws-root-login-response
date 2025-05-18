# CloudTrail Setup Notes

## Purpose
Capture AWS root login events and send them to EventBridge.

## Trail Settings

- **Trail Name**: RootLoginTrail
- **Applies to multi-regions**: Enabled 
- **Management events**: Read + Write
- **Data events**: Disabled
- **S3 Bucket**: root-login-logs-oluwatobi
- **Status**: Enabled
- **SNS Delivery**: Not enabled (I'll handle this manually later)
