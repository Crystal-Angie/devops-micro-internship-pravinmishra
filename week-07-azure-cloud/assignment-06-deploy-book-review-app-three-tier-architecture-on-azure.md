# Assignment 6 — Capstone: Deploy Book Review App (Three-Tier Architecture) on Azure

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a production-ready, best-practice-compliant three-tier architecture on Azure: separated presentation, application, and database tiers, least-privilege network access, a controlled public entry point, protected secrets, and availability/monitoring evidence.

---

# Task 1 — Design the Azure Three-Tier Architecture

## Goal

Create an architecture diagram and implementation plan identifying the presentation, application, and database components, the chosen Azure services, the public entry point, and the internal traffic paths.

### Evidence

#### Screenshot 1 — Architecture diagram showing the public entry point, three tiers, network boundaries, and traffic flow

Add your screenshot here.

---

#### Screenshot 2 — Written architecture assumptions and selected Azure services

Add your screenshot here.

---

# Task 2 — Create the Azure Network Foundation

## Goal

Create a dedicated Resource Group and VNet with separate subnets for the web, application, and database tiers, keeping the application and database tiers without direct public access.

### Evidence

#### Screenshot 3 — Resource Group overview showing the assignment resources

[Assignment screenshot](screenshots/week7-6c.png)

---

#### Screenshot 4 — VNet overview showing the address space and all required subnets

[Assignment screenshot](screenshots/week7-6d.png)

---

#### Screenshot 5 — Route-table or Private DNS evidence where applicable

[Assignment screenshot](screenshots/week7-6e.png)

---

# Task 3 — Configure Security and Secret Management

## Goal

Apply least-privilege NSG rules so traffic flows Internet → public entry point → web tier → application tier → database tier, and store credentials in Azure Key Vault or another approved secure mechanism.

### Evidence

#### Screenshot 6 — NSG rules proving least-privilege access between the tiers

DB Tier: Allow only App Tier NSG traffic on port 3306
[Assignment screenshot](screenshots/week7-6f.png)

App Tier: Allow only Web subnet NSG traffic on port 3001 & 22(this was to allow for jump host between web-app vm, since I wasn’t using bastion)
[Assignment screenshot](screenshots/week7-6g.png)

Web Tier(web ec2 security group): Allows HTTP (80) & SSH
[Assignment screenshot](screenshots/week7-6g2.png)
---

#### Screenshot 7 — Key Vault or approved secret-management configuration (without displaying secret values)

[Assignment screenshot](screenshots/week7-6h.png)

---

# Task 4 — Deploy the Presentation (Web) Tier

## Goal

Deploy the Book Review App presentation layer on the approved web-tier compute service, configured to route requests to the internal application-tier endpoint, and not directly exposed except through the public entry service.

### Evidence

#### Screenshot 8 — Web-tier compute overview showing subnet and availability configuration

[Assignment screenshot](screenshots/week7-6i.png)

---

#### Screenshot 9 — Terminal or service output proving the presentation layer is running

[Assignment screenshot](screenshots/week7-6j.png)

---

# Task 5 — Deploy the Business (Application) Tier

## Goal

Deploy the Book Review App backend privately in the application subnet, configured to use the private database endpoint and secured environment values, reachable only through its internal endpoint.

### Evidence

#### Screenshot 10 — Application-tier compute overview showing private subnet placement

[Assignment screenshot](screenshots/week7-6k.png)

---

#### Screenshot 11 — Backend process, service, or listening-port evidence

[Assignment screenshot](screenshots/week7-6l.png)
---

#### Screenshot 12 — Internal health-check or API response (without exposing secrets)

[Assignment screenshot](screenshots/week7-6m.png)

---

# Task 6 — Deploy the Managed Database Tier

## Goal

Create a private Azure managed database (public access disabled), with availability/backup/retention settings, the Book Review App schema imported, and access restricted to the application tier only.

### Evidence

#### Screenshot 13 — Database overview showing private connectivity and public access disabled

[Assignment screenshot](screenshots/week7-6m.png)

---

#### Screenshot 14 — Availability, backup, and retention configuration

[Assignment screenshot](screenshots/week7-6n.png)

---

#### Screenshot 15 — Successful schema or connectivity verification (without exposing credentials)

[Assignment screenshot](screenshots/week7-6l.png)

---

# Task 7 — Configure Traffic Management, Availability, and Monitoring

## Goal

Configure the approved public entry service with health probes and backend pools, internal routing for the application tier where required, and enable Azure Monitor/diagnostics/logs/alerts for the key resources.

### Evidence

#### Screenshot 16 — Public entry service showing listener, frontend endpoint, and healthy web targets

[Assignment screenshot](screenshots/week7-6o.png)

A public LB for the frontend in Web Tier(subnet) with web VM configured as backed pool

---

#### Screenshot 17 — Internal application-tier load-balancing or routing configuration where applicable

[Assignment screenshot](screenshots/week7-6p.png)

An internal LB for the app servers in app subnet with app vm as backend pool and private frontend ip only accessible/used by the web vm to send requests to app vm

---

#### Screenshot 18 — Azure Monitor, diagnostic settings, logs, metrics, or alert evidence

[Assignment screenshot](screenshots/week7-6q.png)

---

# Task 8 — Validate the Production-Style Deployment

## Goal

Confirm the Book Review App works end to end through the public endpoint, with at least one database read and one write, confirm private tiers are not internet-reachable, and complete a safe availability test.

### Evidence

#### Screenshot 19 — Browser showing the Book Review App through the public endpoint

App accessible via Public-LB dns & IP, with books showing

[Assignment screenshot](screenshots/week7-6r.png)

---

#### Screenshot 20 — Proof of successful database-backed read and write operations

[Assignment screenshot](screenshots/week7-6s.png)

---

#### Screenshot 21 — Evidence that private tiers are not publicly accessible

[Assignment screenshot](screenshots/week7-6e.png)
[Assignment screenshot](screenshots/week7-6f.png)

---

#### Screenshot 22 — Availability-test and healthy-target evidence

[Assignment screenshot](screenshots/week7-6n.png)

---

#### Public Endpoint

Paste your public endpoint URL here:

`http://bookreviewapp.centralus.cloudapp.azure.com `

---

### Notes

Summarize what worked, issues encountered and how they were fixed, and the availability/security/secrets/monitoring/backup choices made.

I didn’t encounter much issues having previously done the deployment on AWS and a similar one on azure too, only the networking configuration(NSG- inbound and outbound) was a bit confusing due to azure differences to AWS, sought  help in the group and sorted it out.

Secrets Management: DB credentials were stored inside Azure Key Vault using Azure RBAC access policies.

Availability & Traffic Management: A HTTP health probe monitoring was configured on  port 80 every 5 seconds to query targets health. DB was configured in different availability zones with enabled zone redundancy to ensure high availability.

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, keys, connection strings, or subscription IDs

---

# Completion Checklist

- [ ] Task 1: Architecture diagram and assumptions documented (Screenshots 1–2)
- [ ] Task 2: Network foundation created with isolated tiers (Screenshots 3–5)
- [ ] Task 3: Least-privilege security and secret management configured (Screenshots 6–7)
- [ ] Task 4: Presentation tier deployed (Screenshots 8–9)
- [ ] Task 5: Application tier deployed privately (Screenshots 10–12)
- [ ] Task 6: Managed database tier deployed privately (Screenshots 13–15)
- [ ] Task 7: Public entry, internal routing, and monitoring configured (Screenshots 16–18)
- [ ] Task 8: End-to-end validation and availability test completed (Screenshots 19–22, Public Endpoint, Notes)
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
