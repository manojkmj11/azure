# Azure Services

## 1. Azure App Services
**What it is:** Fully managed platform for hosting web apps (UI), APIs (optional, mobile backends).

**Real-world example:** 
- "I deployed a Node.js e-commerce API using App Services. It auto-scaled during Black Friday traffic spikes from 2 to 10 instances, handled 10K requests/sec, and I didn't manage any servers. It integrated with my CI/CD pipeline for auto-deployment from GitHub."

**cw-project:** 
- "I worked with a .Net API (using ASP.NET Web API) code base and front end Angular code base (**Apps.Pricing.Analyser, Api.Npr.ContractPayor, UI-trisus**) which used App Services. It was configured to auto-scale when needed. it can handle upto ~10K requests/sec, and i didnt even manage any servers. It integrated with azure CI/CD pipleline for auto-deployment from azure repos."
- optionally mention this, "APIs, UIs were deployed for 3 different environments (DEV, QA, PROD) and each of them were configured with different resources allocation (RAM, CPU, Storage).
- DEV ENV had very limited resources allocated (less RAM - less than 1 or 2GB, CPU - basic processors, 1-2 cores/2 threads) since, its only used by DEVs for feature testing"
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
- "I stored around ~100GB of raw JSON data in Blob Storage which helped for backend systems (ADF uses contract data) to process which uses un-structed data (you uploaded raw json contract data into blob for revenue lookback feature)"
- "I stored around ~50GB worth of user uploaded CSV schedule files (when you upload any schedule files in RM product it gets uploaded to blob storage) in Blob Storage with tiered pricing-hot tier for recent files, cool tier for archives. Which resulted in ~20% cost reduction."
- optional, "Data storage cost was further reduced by compressing (reducing the size) the data before uploading (and de-compress it before using) (NPR-Detail, NPR-Summary uploaded by customer are compressed and then uploaded to reduce the file size. since, if you use less storage azure will charge you less)" 