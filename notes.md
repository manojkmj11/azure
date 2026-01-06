# Azure Services

## 1. Azure App Services
**What it is:** Fully managed platform for hosting web apps (UI), APIs (optional, mobile backends).

**Real-world example:** 
- "I deployed a Node.js e-commerce API using App Services. It auto-scaled during Black Friday traffic spikes from 2 to 10 instances, handled 10K requests/sec, and I didn't manage any servers. It integrated with my CI/CD pipeline for auto-deployment from GitHub."

**cw-project:** 
- "I worked with a .Net API (using ASP.NET Web API) code base and front-end Angular code base (**Apps.Pricing.Analyser, Api.Npr.ContractPayor, UI-trisus**) which used App Services. It was configured to auto-scale when needed. It can handle up to ~10K requests/sec, and I didn't even manage any servers. It integrated with Azure CI/CD pipeline for auto-deployment from Azure repos."
- Optionally mention this: "APIs, UIs were deployed for 3 different environments (DEV, QA, PROD) and each of them were configured with different resource allocation (RAM, CPU, Storage).
- DEV ENV had very limited resources allocated (less RAM - less than 1 or 2GB, CPU - basic processors, 1-2 cores/2 threads) since it's only used by DEVs for feature testing."
- "QA ENV, had slight bumped up resources which is used for end-to-end and performance testing (RAM - more than 4 gigs, CPU - mid-tier processors, 2-4 cores/4-8 threads)"
- "PROD ENV, had the best resources allocation since, this version would be used by actual users with real-world data. (RAM - more than 4 gigs, CPU - high-end processors, more than 4-cores/8-16 threads)"

**Key benefit:** No infrastructure management (it will be managed by azure), built-in scaling (increasing/decreasing instances. although, you can still manage it manually), integrated DevOps (integrating azure repo with azure CI/CD).

---

## 2. Azure Storage Service
**What it is:** Massively scalable cloud storage with three main components:

### a) Blob Storage
**Use case:** Store unstructured data (images, videos, documents, JSON content, CSV/excel files)

**Interview example:** "In my file management system, I stored 500GB of user-uploaded documents in Blob Storage with tiered pricing—hot tier for recent files, cool tier for archives. Reduced storage cost by 40%."

**cw-project:**
- "I stored around ~100GB of raw JSON data in Blob Storage which helped backend systems (ADF uses contract data) process unstructured data (you uploaded raw JSON contract data into blob for revenue lookback feature)."
- "I stored around ~50GB worth of user-uploaded CSV schedule files (when you upload any schedule files in RM product it gets uploaded to blob storage) in Blob Storage with tiered pricing—hot tier for recent files, cool tier for archives. This resulted in ~20% cost reduction."
- Optional: "Data storage cost was further reduced by compressing (reducing the size) the data before uploading (and decompressing it before using) (NPR-Detail, NPR-Summary uploaded by customers are compressed and then uploaded to reduce the file size. Since if you use less storage, Azure will charge you less)." 

### b) Table Storage
**Use case:** NoSQL key-value store for semi-structured data (e.g. JSON data)

**Interview example:** "Used Tables to store IoT sensor readings from 1000 devices. Simple query by device ID, great for time-series data without heavy querying needs."

**cw-project**:
- "Used Tables only internally by Azure Function App (ADF) to store orchestration details like instance ID, start time, status, etc. (When you click on ***Build***, ***Apply Analysis for PA and FA*** for any ***session, price model***, it will start an orchestration within ADF)."


### c) Queue Storage
**Use case:** Async message queue for decoupling services (breaking down large repos into smaller ones)

**Interview example:** "Built an order processing system: frontend queues orders, backend workers process them asynchronously. Handled 5K orders/day without blocking user experience."

**cw-project**:
- ***Step 1:*** "Frontend (UI.Trisus) queues build requests using a backend API (Apps.Pricing.Analyser) and the backend API creates a new queue message and pushes it into the Azure Queue."
- ***Step 2:*** "Azure Function App is configured to read the Queue messages using Queue Trigger and starts orchestration."
- "Hence, using Queue Storage, we were able to split the backend systems into small components. (We have a separate repo for API and a separate one for ADF) which helped in development and maintenance, and they can scale independently."
- "Handled large volume of data processing ranging from 500K to 5M claim lines without blocking user experience (when the session builds, the UI isn't frozen, you can navigate to different pages and such)."

---

## 3. Azure SQL Server & Database
**What it is:** Managed relational database (PaaS - Platform as a service) with Azure SQL Server.

**Real-world example:**
"Migrated legacy on-prem database to Azure SQL. Set up auto-backups, geo-redundancy, and read replicas. Used elastic pools for multi-tenant app—reduced licensing cost by 50% while improving availability from 99.5% to 99.99%."

**cw-project**:
- "Within the whole Trisus application we widely use Azure SQL to store and manage a wide range of data which includes user data, temporary data for backend systems, and also session review, pricing analysis, and fee schedule analysis results and their related configurations."
- "Migrated from ES (Elasticsearch) to Azure SQL to store claim data which helped apply proper schema to data while improving availability to 99% and maintainability of data."

**Key benefit:** Zero maintenance, automatic backups, built-in security, elastic scaling.

---

## 4. Azure Service Bus
**What it is:** Enterprise messaging service for reliable, asynchronous communication between apps (e.g. between Pricing Analyser and ADF).

**Real-world example:**
"Built a notification system for an e-commerce platform: Order service publishes events to Service Bus topics, and Email/SMS services subscribe independently. If Email service is down, messages queue up—no data loss. Also implemented dead-letter queues for failed messages."

**cw-project:**
- "Made use of Azure Service Bus to notify apps whenever new claim data was uploaded by customers."
- "Used Azure Function app to publish messages whenever there is a new file uploaded by customers via SFTP (Secure File Transfer Protocol) servers using FileZilla Windows app." 
- "The Service Bus message included details such as: FileName, StoragePath, DateTimeOfUpload, etc., and it was just simple JSON content."
- "We made use of Service Bus Trigger within an Azure Function App to subscribe to notifications/messages and deserialize it to extract the details for further processing."

**Key benefit:** Guaranteed delivery, pub-sub (publisher-subscriber) pattern, FIFO (First In First Out) ordering, service decoupling (helps in breaking down large code bases (repos) into smaller independent ones).

---

## 5. Azure Key Vault
**What it is:** Secure secret management service (API keys, DB passwords, certs, encryption keys).

**Real-world example:**
"Stored database connection strings, API keys, and SSL certificates in Key Vault. My app authenticates with managed identity (zero credential storage). Rotated API keys monthly—Key Vault handled versioning automatically. Audit logs showed who accessed which secret and when."

**cw-project:**
- "Stored DEV & QA users' secret key in Key Vault and it was integrated with our mail IDs and both were integrated within Visual Studio (purple one)."
- "This was done mainly for executing tests within automation code bases (SpecFlow, UI automation) since UI App needed user secret to securely login to the DEV/QA/Prod ENVs."
- "When a DEV/QA has logged in with his/her mail ID in Visual Studio and executes a test case, we internally make calls to Azure Key Vault using the azureKeyVault library passing the user mail ID and correct URL to fetch the secret key which would enable the user to securely execute tests."

**Key benefit:** Centralized security (you can make a group and assign the same key to everyone in the same team), no hardcoded secrets, audit trails (You can check who and when a secret key was used), automatic rotation (the secret key can be configured such that it can be reset after a period of time, just like we reset our password).

---

## 6. Azure Functions
**What it is:** Serverless compute—run code without managing servers, pay only for execution time. Triggered by events (HTTP, timers, queues, blobs, Service Bus, etc).

**Real-world example:**
"Built image processing pipeline: when users upload photos, a blob trigger fires a Function automatically to resize/compress images within 5 seconds. Scaled from 1 to 1000 concurrent executions during upload surge. Cost: $10/month instead of $500/month for dedicated VMs."

**cw-project:**
- "Used Azure Function App (ADF) as the backend orchestration engine for Pricing Analyser (PA) and Fee Analysis (FA). When users trigger 'Build' or 'Apply Analysis', Queue Storage messages trigger the Function to start orchestrations."
- "Functions read messages from Azure Queue, deserialize them to extract metadata (SessionID, ModelID, etc.), and kick off orchestrations processing 500K-5M claim lines without blocking the UI."
- "Also used Service Bus Trigger within Functions to listen for new claim file uploads from customers via SFTP. When triggered, Functions deserialize the message and process the uploaded data (validation, transformation, storage)."
- "Timer Trigger: Scheduled Functions to run daily cleanup jobs—archiving old session data, compressing old JSON files in Blob Storage, and purging stale Table entries. Freed up 30GB storage monthly."
- "Functions integrated with Azure SQL to read configurations and write processing results. Zero infrastructure overhead—auto-scaling handled spikes in parallel orchestrations."

**Key benefit:** No server management (Azure handles scaling, patching, provisioning), pay-per-execution (cost-efficient), event-driven (reacts to queues, blobs, HTTP, timers), quick deployment (deploy in seconds).

---

## Interview Tip
When asked about these services, always mention:
- **Problem solved** (scalability, maintenance, security)
- **Scale/metrics** (users, requests, data size)
- **Business impact** (cost savings, uptime improvement)
- **Why this service** (not alternatives)
