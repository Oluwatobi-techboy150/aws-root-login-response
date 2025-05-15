# How I Set Up SNS Alerts for AWS Root Account Login

To get immediate alerts when the AWS **root account** is used, I manually created an SNS topic and subscribed my email. Here's how I did it step-by-step using the AWS Console.

---

##  Step 1 — Creating the SNS Topic

1. I went to the **Amazon SNS Console** and clicked on **Topics**.
2. I selected **Create topic** → chose **Standard**.
3. I entered the topic name: RootLoginAlertTopic
4. I left the rest of the settings as default.
5. I clicked **Create topic** and it was successfully created.

---

##  Step 2 — Subscribing My Email

1. After the topic was created, I clicked **Create subscription**.
2. Under **Protocol**, I chose Email.
3. For **Endpoint**, I entered my personal email address.
4. I clicked **Create subscription**.
5. I went to my email inbox and **confirmed** the subscription using the link AWS sent me.

---

##  Status Update

- SNS Topic: RootLoginAlertTopic → Created  
- Email: Subscribed and confirmed  
- Ready to be connected to the **EventBridge rule** in the next step.
![IMG_7124](https://github.com/user-attachments/assets/56a20087-d8ec-4fa9-ab9e-ce0afa876b59)
---

##  The Reason Why I Did This:

Since using the root account is a high-risk activity, I wanted to be notified immediately if it happens. This setup lets me respond quickly as a SOC Analyst in my simulation project.

Later, I’ll connect this SNS topic to an EventBridge rule that listens for root sign-in events from **CloudTrail logs**.

---

## What I Will Be Doing Next:

- [ ] Create an EventBridge rule to detect root login  
- [ ] Set it to trigger this SNS topic  
