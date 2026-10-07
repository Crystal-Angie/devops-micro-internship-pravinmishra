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

[Assignment screenshot](screenshots/week6-6a.png)

---

# Task 2 — AWS Region & Services Used

## Goal

Record the AWS Region used and list every AWS service used across networking, compute, load balancing, security, and the database.

### Notes

**Region:**

Region: I used “use1-az1 (us-east-1a) &  ‘use1-az2 (us-east-1b)”

---

**Services:**

Services: The architecture I provisioned uses AWS VPC networking, multi-AZ load balancing, isolated compute tiers, subnets, security groups and a managed Multi-AZ RDS backend to achieve scalability, security, and high availability.

AWS Services I Used Include;
- VPC
- Subnets
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Amazon RDS(MySQL) & Read Replica
- Application Load Balancers & Target grroups
- Elastic compute cloud (Web & App EC2)


---

# Task 3 — Public Entry Point

## Goal

Confirm the Book Review App loads through the public ALB DNS name.

### Evidence

#### Public ALB DNS

Paste your public ALB DNS name here:

`http://cap-pub-alb-885037544.us-east-1.elb.amazonaws.com `

---

# Task 4 — Evidence Screenshots

## Goal

Capture visual proof of every tier and load balancer.

### Evidence

#### Web EC2

[Assignment screenshot](screenshots/week6-6b.png)

---

#### App EC2

[Assignment screenshot](screenshots/week6-6c.png)

---

#### Public ALB

[Assignemnt screenshot](screenshots/week6-6d.png)

---

#### Internal ALB

[Assignment screenshot](screenshots/week6-6e.png)

---

#### RDS + Replica

[Assignment screenshot](screenshots/week6-6f.png)

---

#### App UI proof

[Assignment screenshot](screenshots/week6-6g.png)

---

# Task 5 — Summary

## Goal

Summarize what worked in the final deployment, the issues encountered and how each was fixed, and the tools or sources used to research and debug.

### Notes

**What worked:**

The VPC was correctly subnetted across multiple Availability Zones, with public subnets hosting Public Application Load Balancers & EC2(Web) and private subnets hosting App EC2 instance, Internal ALB and the RDS database.
The frontend was served through Nginx, the backend Node.js (Express) application was deployed on EC2 and managed with PM2, and connectivity to the RDS MySQL database was confirmed. 
Security Groups and routing were correctly configured to allow controlled traffic flow between the web, application, and database tiers.
App was accessible via public ALB, registration and login also worked perfectly confirming everything was correctly set up and healthy.


---

**Issues + fixes:**

- PM2 startup and port conflicts: Multiple PM2 processes caused port clashes and startup issues. These were fixed by cleaning existing PM2 processes, saving the correct process list, and enabling PM2 startup with systemd.

- Reverse proxy and API path mismatch: Nginx and frontend requests initially pointed to incorrect API paths causing books not to show even though application was accessible on public-ALB DNS. Aligning api route mounting to the backend with the Nginx proxy configuration resolved the issue.

---

**Tools/sources used:**

AI (ChatGPT), Notes and help from Group mates.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post sharing the capstone deployment, including the public ALB DNS (or a redacted screenshot), three to five lines on what you built and why it is production-style, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/angela-chibuike_aws-threetierarchitecture-highavailability-activity-7432165106173743104-IdQ-?utm_source=share&utm_medium=member_desktop&rcm=ACoAADn-PSABhIre4cnftTYXk433XaYMG-l_k9Y `

---

#### Screenshot of LinkedIn post

[Assignment screenshot](screenshots/week6-6h.png)

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, RDS credentials, connection strings, private keys, or account IDs

---

# Completion Checklist

- [ ] Task 1: Architecture diagram completed
- [ ] Task 2: AWS Region and services documented
- [ ] Task 3: Public ALB DNS confirmed working
- [ ] Task 4: All six evidence screenshots captured (Web Tier, App Tier, both ALBs, RDS + replica, app UI)
- [ ] Task 5: Deployment summary completed (what worked, issues/fixes, tools/sources)
- [ ] LinkedIn post published and URL submitted
- [ ] App Tier and Database Tier confirmed not publicly accessible
- [ ] No sensitive data exposed

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