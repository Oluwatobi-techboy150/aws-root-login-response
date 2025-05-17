# EventBridge Rule: Detecting Root Account Login

## Objective

The goal of this phase was to create a real-time detection mechanism using Amazon EventBridge to catch any AWS Management Console login using the root account, and trigger an SNS email alert immediately. This improves visibility and reduces the risk of unnoticed privileged access.

## Tools & Services Used

- **AWS CloudTrail** – To log the root account login.
- **Amazon EventBridge** – To match login events and trigger actions.
- **Amazon SNS** – To send alert emails when the rule matches.
- **IAM** – For SNS permissions and EventBridge execution.

## Step-by-Step Implementation

###  Step 1: CloudTrail Setup (Preconfigured)

I had already enabled CloudTrail to log management events across all regions. This step ensured all console login activity was recorded, including root account usage.

- Trail name: RootActivityTrail
- Multi-region: Yes
- Management Events: All(I checked Read and Write)
- Log destination: S3
- Log validation: Enabled

###  Step 2: Create SNS Topic and Subscribe Email

- Go to Amazon SNS → Topics → Create topic.
- Choose Standard.
- Name it: RootLoginAlertTopic
- Copy the ARN: arn:aws:sns:us-east-1:xxxxxxxxxxxx:RootLoginAlertTopic
- Go to **Subscriptions** → Create subscription.
- Protocol: Email, Endpoint: your email (e.g., myemail@example.com)
- Confirm the subscription from your inbox.
  
![confirmation email for SNS topic subscription](https://github.com/user-attachments/assets/e4533f01-6f86-493a-9a0b-c324973d6f5c)

### ✅ Step 3: Create the EventBridge Rule

- Go to EventBridge → Rules → Create Rule
- Rule name: RootloginDetector
- Event Bus: default
   _(I used the default bus since it handles AWS service events by default)_
- Rule type: Standard rule
- **Paste this JSON under “Event Pattern”:**

 {
  "source": ["signin.amazonaws.com"],
  "detail-type": ["AWS Console Sign In via CloudTrail"],
  "detail": {
    "userIdentity": {
      "type": ["Root"]
    },
    "responseElements": {
      "ConsoleLogin": ["Success"]
    }
  }
}



![JSON event pattern used in the EventBridge rule](https://github.com/user-attachments/assets/64e3701a-34ca-4ebc-a2ca-a7e6e3d0acc6)

## SNS email alert I got after I tried to loging to my AWS console. 

![SNS email Alert](https://github.com/user-attachments/assets/6f025bf2-7f8c-4bf9-93cf-2d506c7dce82)
)

_(Account number blurred for security)_

## Event history on CloudTrail after I tried to logging to my AWS console. 

![Event history on CloudTrail](https://github.com/user-attachments/assets/59cf3f4c-b4fc-4705-bc9d-bcc17679a6bb)

_It gave me the details of the ConsoleLogin_

These screenshots confirm that my configuration worked as expected.


## Lessons Learned

- I encountered some issues while setting this up. EventBridge wasn’t triggering at first, and I couldn’t select the SNS topic until I confirmed the email subscription. It reminded me how one small misstep can break the whole chain.

- Testing a real login from another device gave me confidence that everything worked. This phase taught me how tightly connected each AWS service is — and how important it is to test and verify, not just configure.

## Reflection

 This felt like real work. I wasn’t just setting up services. I was thinking through a detection and response flow like a security analyst.

 It’s one thing to know AWS services on paper. It’s another to build, troubleshoot, and see alerts land in your inbox. This gave me real confidence in my skills.

 I'll be moving on to the Lambda auto-response now.
