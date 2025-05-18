## AWS-root-login-response

Real-time AWS root login detection + response

---

### Overview

This project detects and responds to AWS root account usage in real time using:

* **CloudTrail** for logging API calls
* **EventBridge** for triggering actions
* **Lambda** for lightweight auto-response
* **SNS** (configured for alerts, though not used in final response)  
* **S3** (implemented in a previous project for evidence capture)


---

## Phase 1: Detection with CloudTrail + EventBridge

I started by enabling CloudTrail and configuring it to log all management events, including both read and write activity. Then I set up Amazon EventBridge to monitor for the ConsoleLogin event, specifically when the user identity is the root account.

This event pattern triggers a rule that forwards the event to a target, which in this case is an AWS Lambda function that performs the automated response.

---

## Phase 2: Auto-Response with AWS Lambda

Once I had reliable detection in place, I built a lightweight auto-response system using **AWS Lambda**.

The function is triggered by EventBridge when a root login is detected. It parses the event and prints the following:

* The root user's ARN
* The source IP address
* The login timestamp

While I initially planned to send an email alert using SNS and store the event in S3, I chose to pause here:

> Since SNS was already configured earlier in this project, and I implemented S3 evidence capture in a previous simulation, I decided to avoid duplication. Instead, I’m documenting this phase cleanly before expanding further.

This gives me a working detection + response pipeline that I can build on later.

