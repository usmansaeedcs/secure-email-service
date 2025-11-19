# secure-email-service

## Solution Design

![Solution Design](https://github.com/usmansaeedcs/secure-email-service/blob/main/email-app-hld.jpg)

## Project Functional Requirements

- Secure User Login
- Read Emails from Inbox 
- Send Emails 
- Store Attachments – not part of scope but felt it was essential
- User Session Management – We want to timeout, refresh and logout user sessions as a good security practice 
- API Gateway/Backend – We want backend secure APIs to fetch, send and store. this works best with the FE app.
- Language – React FE JavaScript, TypeScript or GoLang backend. Its lightweight, easy to read and learn.

## SRE Non-Functional Requirements

### Availability 
Tracking High Uptime SLOs, Having Multiple Avilabily Zones to ensure we can spin up in another region,. A Disaster Recovery strategy in place, for example what if the DB goes down? do we have a snapshot to restore, or do we replicate to a secondary region? Decoupling queuing cache using SQS and Elasticache as you’ll see in the next slide, this design decision was made to ensure we decouple components for retry if the DB or Mailing Service is down. it should also give us a gradual failure if said systems go down completely by alerting if queue size exceeds X count.  This also covers the scalability principle as well as availiability.

### Scalability 
Auto-scaling of API services, DB auto-scaling, and caching/queuing to support growth. More of that will be clear in the next slide
Observability – SLIs, SLOs, and dashboards (CloudWatch, DataDog), structured logging. more on what that looks like in the next slide

### Security 
Encryption at rest and in transit - Cloudfront CDN will encrypt TLS in transit to the FE, KMS will encrypt at rest in the backend components. IAM least privilege, Web Application Firewall from the DNS and Cognito Authentication token from user login that will be used to authenticate backend APIs also.

### Cost Efficiency
Using managed services like AWSs Simple Email Service, reduces backend development. Also a Static FE React app stored in S3, and a serverless event-driven backend that would reduce cloud costs significantly. More on that in the next slide

### Maintainability 
Infrastructure as Code (Terraform), CI/CD pipelines like GitLab

### Reliability 
Automated backups of the the DB, cross-region replication for critical data like the email attachments store in s3, error budget policies so we can better measure reliability in a quantative way - ensuring we adjust release velocity when budget is spent

### Incident Response 
PagerDuty integration, runbooks, on-call processes, and tracking our MTTR
