# Architecture Overview
![641125DD-EABB-4575-817B-A1E262D738FF](https://github.com/user-attachments/assets/e33264c9-8d6d-4c9e-84c7-788aaae99b5d)

This project is a simple, serverless setup that watches for root logins and reacts quickly.

### Key parts:

- **CloudTrail** logs all management actions, including root logins.  
- **EventBridge** listens for root ConsoleLogin events and triggers the next step.  
- **Lambda** runs automatically when a root login happens. It grabs important info and logs it.  
- **SNS & S3** are set up in other parts or projects — they can be plugged in later for alerts and evidence storage.  

### How it works:

1. Root logs in.  
2. CloudTrail records the login event.  
3. EventBridge spots the event and triggers Lambda.  
4. Lambda pulls details like the root user’s ARN, IP, and login time.  
5. Next steps (alerts, archiving) can be added later without messing up the core flow.  

### Why this design?

- Keeps things simple and modular.  
- Focuses on clear visibility before adding automation.  
- Builds understanding by doing and documenting everything step-by-step.  

