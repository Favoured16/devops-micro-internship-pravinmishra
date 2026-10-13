# Assignment 6 — Capstone Assignment — Deploy Book Review App (Three-Tier Architecture) on AWS

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a fully production-style three-tier architecture on AWS: a Next.js Web Tier behind Nginx and a public ALB, a private Node.js/Express App Tier behind an internal ALB, and a private Multi-AZ MySQL RDS database with a read replica. You are expected to design, deploy, isolate, debug, and document the result independently.

---

# Task 1 — Architecture Diagram

## Goal

Create an architecture diagram showing the custom VPC (10.0.0.0/16), the six subnets across two Availability Zones (two public Web Tier, two private App Tier, two private Database Tier), the public ALB, Web Tier EC2/Nginx, internal ALB, private App Tier EC2, private Multi-AZ RDS with its read replica, and the permitted traffic flow.

### Evidence

#### Diagram image or link

![alt text](<Architecture Diagram showing 6b subnet with their AZ-1.png>)

---

# Task 2 — AWS Region & Services Used

## Goal

Record the AWS Region used and list every AWS service used across networking, compute, load balancing, security, and the database.

### Notes

**Region:**

AWS Region

US East (N. Virginia) — us-east-1

---

**Services:**

AWS Services Used

Networking

VPC — custom VPC book-review-vpc 
Subnets — 6 subnets across 2 AZs (2 public, 2 private-app, 2 private-DB)
Internet Gateway — provides internet access to the public subnets
NAT Gateway — provides outbound internet access for the private app-tier subnets 
Route Tables — separate routing for public subnets (via Internet Gateway) and private subnets (via NAT Gateway)

Compute

EC2 — application/web servers running in the app tier (and/or web tier, depending on your architecture)
Auto Scaling Group (ASG) — maintains desired capacity and replaces unhealthy instances automatically across both AZs
Launch Template — defines the AMI, instance type, security group, and user-data script used to bootstrap each instance

Load Balancing

Application Load Balancer (ALB) — public-facing, internet-facing, terminates HTTP/HTTPS and routes to the web tier
Internal ALB — routes traffic privately from the web tier to the app tier

Security

Security Groups — one per tier (ALB, web/app, DB) enforcing least-privilege access between tiers
IAM — roles/permissions for resource access (e.g. instance roles, if used for SSM or other AWS API access)

Database

RDS (Multi-AZ) — private, non-publicly-accessible relational database with a primary instance and a synchronously-replicating standby/read replica across the two AZs

A couple of gaps worth confirming before finalizing this for your report:

Whether NAT Gateway has actually been created yet in this new setup (we've only done VPC + subnets so far)
Whether IAM roles are attached to instances in this new build (recall the earlier ha-project had none, which caused SSM access issues)
The exact RDS engine (MySQL, PostgreSQL, etc.) for the database service entry

---

# Task 3 — Public Entry Point

## Goal

Confirm the Book Review App loads through the public ALB DNS name.

### Evidence

#### Public ALB DNS

Paste your public ALB DNS name here:

Book-Review-Web-ALB-1467519084.us-east-1.elb.amazonaws.com

---

# Task 4 — Evidence Screenshots

## Goal

Capture visual proof of every tier and load balancer.

### Evidence

#### Web EC2

![alt text](<Web EC2-1.png>)

---

#### App EC2

![alt text](<App EC2.png>)

---

#### Public ALB

![alt text](<ALB details showing two public subnets in two Availability Zones-1.png>)

---

#### Internal ALB

![alt text](<Internal ALB.png>)

---

#### RDS + Replica

![alt text](<RDS + Replica.png>)

---

#### App UI proof

![alt text](<App UI proof.png>)

---

# Task 5 — Summary

## Goal

Summarize what worked in the final deployment, the issues encountered and how each was fixed, and the tools or sources used to research and debug.

### Notes

**What worked:**

**What worked:**

 The Next.js frontend was successfully deployed and served through Nginx on the frontend EC2 instance.
 The backend Node.js/Express API was successfully deployed and managed with PM2.
 The Internal Application Load Balancer successfully routed API requests to the backend EC2 instance.
 The frontend successfully communicated with the backend through Nginx and the internal ALB.
 The database connection and Sequelize models worked successfully.
 The books API successfully returned and displayed book data.
 User registration successfully worked when tested directly through the backend, ALB, and Nginx.
 CORS was identified and configured to allow requests from the deployed frontend.
 PM2 successfully managed the application processes and allowed the frontend and backend services to be restarted safely after configuration changes.


---

**Issues + fixes:**

Frontend displayed “No books available” — corrected the API URL configuration and rebuilt the Next.js application.
Old Next.js JavaScript bundle was still being served — identified PM2 as the process manager and restarted the frontend through PM2 to load the new build.
Registration endpoint initially returned 404 — identified that the correct backend route was /api/users/register, not /users/register.
Browser registration returned HTTP 500 — inspected the browser response and identified a CORS policy error.
Incorrect ALLOWED_ORIGINS configuration — corrected the backend .env entry to allow the deployed frontend origin:
http://100.26.248.127
Multiple PM2 backend entries caused confusion — identified book-review-backend as the active application and used it for backend process management.
Configuration changes were not immediately reflected — restarted the appropriate PM2 application so the updated environment variables were loaded.

---

**Tools/sources used:**

AWS Management Console — EC2, VPC, security groups, Application Load Balancer, target groups, and networking.
Ubuntu/Linux terminal — application configuration, connectivity testing, and troubleshooting.
Nginx — reverse proxy and API routing.
Node.js/Express — backend application and API testing.
Next.js — frontend deployment and build troubleshooting.
PM2 — process management and application restarts.
cURL — testing backend, ALB, and Nginx API endpoints.
Chrome DevTools — inspecting Network requests and identifying the CORS error.
Git/GitHub — source-code and version-control management.
Sequelize — database connection and model management.
Linux commands (grep, sed, ps, pstree) — inspecting configuration files and running processes.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post sharing the capstone deployment, including the public ALB DNS (or a redacted screenshot), three to five lines on what you built and why it is production-style, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

https://lnkd.in/p/eCppqqs4

---

#### Screenshot of LinkedIn post

![alt text](<linkdin post.png>)

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, RDS credentials, connection strings, private keys, or account IDs

---

# Completion Checklist

- [✅] Task 1: Architecture diagram completed
- [✅] Task 2: AWS Region and services documented
- [✅] Task 3: Public ALB DNS confirmed working
- [✅] Task 4: All six evidence screenshots captured (Web Tier, App Tier, both ALBs, RDS + replica, app UI)
- [✅] Task 5: Deployment summary completed (what worked, issues/fixes, tools/sources)
- [✅] LinkedIn post published and URL submitted
- [✅] App Tier and Database Tier confirmed not publicly accessible
- [✅] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*