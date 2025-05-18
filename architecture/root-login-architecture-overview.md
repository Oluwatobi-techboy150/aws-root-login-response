# Architecture Overview

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
