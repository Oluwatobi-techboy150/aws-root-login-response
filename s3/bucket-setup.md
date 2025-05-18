# S3 Bucket Setup 

While S3 is part of my overall design for evidence storage, I did **not re-implement** the setup in this project.

Here's why:

- I already built a full evidence capture workflow using S3 in a previous project.
- The goal of *this* simulation was to focus on **real-time detection and initial response**.
- I wanted to avoid duplicating effort and keep the documentation clean and focused.

But still, the Lambda function is designed to be easily extended, a future version could store the root login event in S3 automatically.

If you'd like to plug in S3:
- Create a secure bucket with versioning and encryption.
- Grant the Lambda function `PutObject` permissions.
- Modify the function to write the event payload to the bucket.

But for now, S3 setup is considered complete based on my earlier implementation.
