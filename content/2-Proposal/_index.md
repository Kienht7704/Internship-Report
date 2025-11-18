---
title: "Proposal"
date: "2025-09-09T19:53:52+07:00"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Aurora Time  
## Unified AWS Serverless Solution for Personal Time Management

### 1. Executive Summary  67890
This proposal presents the implementation plan for Aurora Time, a time management application on the AWS platform, simple and focused on scheduling features, to address the complexity and operational costs of current solutions. Aurora Time will leverage AWS serverless and managed services to ensure high scalability, reliability, and cost optimization, delivering rapid return on investment (ROI) through reduced infrastructure management costs.

### 2. Problem Statement  
#### *Current Problem*  
Individuals struggle to manage daily commitments because schedules are scattered (notes, phones) leading to confusion and missed tasks. Current tools are often too complex, overloaded with work features, and unsuitable for the need for quick and simple scheduling in personal life. Aurora Time solves this by providing a centralized, minimalist, and intuitive platform to easily track habits and important milestones.  

#### *Solution*  
The platform uses Amazon S3 combined with Amazon CloudFront to store and distribute web applications, using AWS Amplify for rapid development and deployment. Amazon API Gateway and AWS Lambda serve as the Backend processing layer to handle event CRUD requests. Data is stored in Amazon DynamoDB to ensure fast access speed and low latency. Amazon Cognito ensures secure authentication and authorization for each individual user. Amazon EventBridge and Amazon SES are used to trigger and send scheduled reminder notifications. Similar to other calendar applications, users can create and edit personal schedules, but this platform operates at a more minimalist scale and serves the purpose of daily personal time management. Key features include intuitive scheduling interface, customizable reminders, and low operational costs.  

#### *Benefits and Return on Investment (ROI)*  
The Aurora Time solution creates a solid foundation for individual users to centralize all schedules while providing a cost-saving Serverless architecture model for easy feature expansion. The platform reduces schedule fragmentation and minimizes missed important commitments through a centralized, simple reminder system, simplifying personal time management and improving work/life balance.

Estimated monthly infrastructure costs of $16 - $50 USD, totaling approximately $192 - $600 USD for 12 months (depending on number of users and requests). All development components are based on Serverless, incurring no hardware purchases or 24/7 virtual server rental costs. The payback period is estimated at under 6 months thanks to significant time savings in searching and manually arranging schedules, and the extremely low operational costs of the AWS Serverless architecture.

### 3. Solution Architecture  
The platform applies AWS Serverless architecture to manage personal schedule and event data, with the capability to easily scale from a single user to millions of individual users. API requests are received through Amazon API Gateway and processed by AWS Lambda, while data is stored in Amazon DynamoDB to ensure fast query speed and low latency. Amazon EventBridge handles reminder scheduling logic, triggers Lambda, and sends notifications. AWS Amplify provides an intuitive web/mobile interface, secured by Amazon Cognito to safely manage access permissions for each user.    

![Aurora Time Platform Architecture](/images/2-proposal/platform_architecture.jpeg)

#### *AWS Services Used*  
- *AWS Lambda*: Processes business logic for event CRUD operations and triggers scheduled reminder tasks.  
- *Amazon API Gateway*: Provides secure RESTful API interface for communication with web applications.  
- *Amazon DynamoDB*: Stores event data, schedules, and user information.  
- *Amazon S3 and CloudFront*: Stores and distributes static content of Frontend applications. 
- *Amazon EventBridge*: Schedules and triggers automatic reminder events at user-defined times.  
- *Amazon SES*: Sends customized reminder notifications via email (SES).
- *AWS Amplify*: Stores and provides intuitive web interface.
- *Amazon Cognito*: Manages access permissions and secure authentication for individual users. 

#### *Component Design*  
- *User Interface*: AWS Amplify hosts web applications (planned to use React) providing intuitive scheduling interface, schedule viewing, and reminder settings.  
- *User Authentication*: Amazon Cognito manages individual user accounts, issuing secure authorization tokens for Backend API access.  
- *API Request Reception*: Amazon API Gateway receives and routes authenticated requests (e.g., create event, query schedule) to corresponding Lambda functions. 
- *Backend Logic Processing*: AWS Lambda processes and executes business logic. Meanwhile, other Lambda functions are triggered by EventBridge to send reminders. 
- *Data Storage*: Amazon DynamoDB stores primary data (Schedules, Events, User Information) in NoSQL format, ensuring fast access and flexibility.  
- *Reminder Scheduling*: Amazon EventBridge schedules and triggers events at user-defined times, ensuring reminder features operate automatically.
- *Web Interface*: AWS Amplify hosts a Next.js app for real-time dashboards and analytics.
- *User Management*: Amazon Cognito manages user access, allowing up to 5 active accounts.

### 4. Technical Implementation  
*Implementation Phases*  
The project consists of 2 parts — Backend Serverless setup and Frontend User Interface building — each part goes through 4 phases:  
1. *Research and Architecture Design*: In-depth research on DynamoDB Data Modeling for schedules and design of verified AWS Serverless architecture (API Gateway, Lambda, EventBridge). (Timeline: Month 1) 
2. *Cost Calculation and Feasibility Check*: Use AWS Pricing Calculator to estimate actual Serverless operational costs and verify feasibility of authentication (Cognito) and storage (DynamoDB) flows. (Timeline: Month 1)  
3. *Architecture Adjustment for Cost/Solution Optimization*: Fine-tune Lambda parameters (e.g., memory, timeout) and DynamoDB (e.g., RCU/WCU) to ensure highest cost efficiency and best performance for individual users. (Timeline: Month 2)  
4. *Development, Testing, Deployment*: Program Lambda functions, set up CI/CD Pipeline with CodePipeline/CodeBuild/CloudFormation, and develop Frontend applications (React). Then conduct Beta testing and go live. (Timeline: Month 2-3)  

*Technical Requirements*  

*1. Backend Requirements (Serverless)*
- *Core Services*: In-depth knowledge of AWS Lambda (Node.js), Amazon DynamoDB, Amazon API Gateway, and Amazon Cognito. 
- *Event Management*: Proficiency in Amazon EventBridge to schedule and trigger Lambda functions sending reminders via Amazon SES/SNS.

*2. Frontend Requirements*
- *Interface*: Practical knowledge of AWS Amplify to host React applications and connect to API Gateway
- *Optimization*: Leverage React's processing capabilities to reduce load on Lambda functions  

### 5. Roadmap & Deployment Milestones  
- *Internship (Month 1–3)*:  
    - Month 1: Learn AWS and upgrade hardware.  
    - Month 2: Design and adjust architecture.  
    - Month 3: Deploy, test, and go live.  
- *Post-Deployment*: Further research over 1 year.  

### 6. Budget Estimation   

*Infrastructure Costs*  
- AWS Amplify: $0.35/month 
- S3 Standard: $0.05/month  
- CloudFront: $1.70/month   
- API Gateway: $0.11/month
- AWS Lambda: $0.00 (free) /month   
- DynamoDB: $0.11/month  
- Amazon Cognito: $0.00/month   
- Amazon SES (email): $0.05/month
- EventBridge: $0.10/month  
- CloudWatch Logs: $0.10/month
- CI/CD Pipeline + Build: $0.00 (free) /month

*Total*: $2.57/month, $30.84/12 months    

### 7. Risk Assessment  
*Risk Matrix*  
- Network disconnection/High latency: Medium impact, medium probability.  
- DynamoDB design error: High impact, medium probability.  
- Serverless costs exceed budget: Medium impact, low probability.
- Reminder system failure: High impact, low probability.

*Mitigation Strategies*  
- Network disconnection/High latency: Optimize Frontend (stored on S3/CloudFront) to ensure fast loading speeds, use Client-side Caching so users can still view recent schedules when network is lost.  
- DynamoDB design error: Conduct strict Proof of Concept (POC) and Load Testing for data models. Use optimized Global Secondary Index (GSI) to avoid querying entire table and minimize RCU/WCU costs.  
- Serverless costs exceed budget: Set up AWS Budgets with proactive alerts (Email/SNS) when spending approaches threshold. Regularly check AWS Cost Explorer and optimize Lambda resources.
- Reminder system failure: Closely monitor EventBridge and Lambda reminder processing through CloudWatch Alarms to detect errors immediately. Implement Retry Logic for critical Lambda functions.

*Contingency Plan*  
- AWS Incident: Since the application is only for individuals, if AWS encounters an incident, the system will be configured to automatically Rollback to the latest code version (via CodePipeline) or re-deploy configuration (via CloudFormation).  
- Data Loss: Set up DynamoDB Backup and Restore at regular intervals to quickly recover event data in case of system failure or user error.
- Cost Issues: Use CloudFormation to restore resource configuration (such as RCU/WCU) to proven low-cost state.

### 8. Expected Results  
*User Experience Improvement*: Provide intuitive scheduling interface and real-time notifications (Real-time notifications) replacing manual note-taking and schedule tracking processes. The system is designed for easy scaling, serving from a single user to millions of individual users.  

*Long-term Value and Reusability*: Create a solid Serverless technical platform that can maintain operation at extremely low costs for many years. This architecture can be reused for personal application projects or other expansion features in the future (e.g., habit tracking, simple personal task management).

*Successful Deployment*: Successfully deploy the entire approved Serverless architecture, including automated CI/CD and core features of Aurora Time within the planned timeframe.