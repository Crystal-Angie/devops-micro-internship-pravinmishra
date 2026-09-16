# Assignment 3 — Production Maintenance Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will treat your already deployed React application (on Ubuntu VM with Nginx) as a live production system. You will perform structured operational checks covering network validation, service health, log analysis, resource monitoring, configuration verification, and incident simulation with recovery — mirroring real on-call DevOps responsibilities.

---

# Task 1 — Server Access & Networking Validation

## Goal

Verify that the deployed React application is reachable from the browser and confirm basic network connectivity of the Ubuntu VM.

### Evidence

#### Screenshot 1 — Browser showing the React app with your Full Name visible on the UI

[Assignment Screenshot](screenshots/image7a.png)

---

#### Screenshot 2 — Output of `ip a`

[Assignment Screenshot](screenshots/image9a.png)

---

#### Screenshot 3 — Output of `sudo ss -tulpen`

[Assignment screenshot](screenshots/image9b.png)

---

#### Screenshot 4 — Output of `sudo ufw status`

[Assignment screenshot](screenshots/image9b.png)

---

### Notes

Answer the following in your own words:

**1. What proves Nginx is listening on 0.0.0.0:80?**

From the screenshot above, we can see the ss output shows 0.0.0.0:80 with Nginx listed as the user/service. This means Nginx is listening on port 80 for incoming connections.

---

**2. What proves SSH is active on port 22?**

From the same screenshot, we can see; tcp LISTEN ... 0.0.0.0:22 ... users:(("sshd"...))

This proves that the SSH service (sshd) is listening and accepting connections on port 22.

---

**3. Did you find any unexpected open ports? Explain briefly.**

No unexpected open ports were found. The main externally accessible ports shown are 80 for Nginx (HTTP) and 22 for SSH, which are expected here. The other listening ports, such as 53, are associated with system services like systemd-resolved and are bound to local/private interfaces rather than being exposed publicly.

---

# Task 2 — Service Health & Systemd Validation (Nginx)

## Goal

Verify that Nginx is properly installed, running, enabled at boot, and safely configured.

### Evidence

#### Screenshot 1 — Output of `systemctl status nginx --no-pager`

[Assignment Screenshot](screenshots/image2b.png)
---

#### Screenshot 2 — Output of `sudo nginx -t`

[Assignment Screenshot](screenshots/image2b.png)

---

#### Screenshot 3 — Output of `sudo ss -lptn '( sport = :80 )'`

[Assignment Screenshot](screenshots/image5a.png)

---

### Notes

Answer the following in your own words:

**1. What happens if Nginx fails to restart in production?**

If Nginx restarts and doesn’t come back, the website would become unavailable, causing downtime for users.

---

**2. What's your basic rollback plan?**

I would restore the previous working configuration and restart Nginx, while i work on fixing the configuration.
---

# Task 3 — Logs & Request Trace

## Goal

Verify real traffic flow and analyze logs to understand system behavior and errors.

### Evidence

#### Screenshot 1 — Output of `sudo tail -n 30 /var/log/nginx/access.log`

[Assignment Screenshot](screenshots/image10a.png)

---

#### Screenshot 2 — Output of `sudo tail -n 30 /var/log/nginx/error.log`

[Assignment Screenshot](screenshots/image10b.png)

---

#### Screenshot 3 — Output of `sudo journalctl -u nginx --no-pager -n 50`

[Assignment Screenshot](screenshots/image10b.png)

---

### Notes

Answer the following in your own words:

**1. Were there any errors in the logs?**

- If yes, mention 1–2 example error lines from the logs and explain what each one means in simple terms.
- If no, explain what it means if the error log is empty or shows no recent errors during your check.

Yes, I saw two  errors in  logs:

directory index of "/var/www/html/" is forbidden
*Meaning:* Nginx tried to show the default directory listing for /var/www/html/, but it’s not allowed (no index.html or permissions issue). This was the first error after installing nginx but was solved by giving permissions.

using inherited sockets from "5;6;"
*Meaning:* This is more like information rather than an error, it's just a notice that Nginx successfully reused existing sockets during a restart.


---

**2. If there were no errors, what does that indicate about the system?**

In the case that there was none, this would indicate that the system has been operating normally and has not encountered any errors in the period covered by the logs.

---

**3. Based on the access logs, were your curl requests visible in the log entries? What does that prove about traffic flow?**

Yes, the curl requests were visible in the access logs. This proves that the requests reached Nginx and that traffic was successfully flowing to the web server.

---

# Task 4 — System Resource Health Check (Capacity Red Flags)

## Goal

Assess server capacity and detect potential performance or failure risks.

### Evidence

#### Screenshot 1 — Output of `uptime`

[Assignment Screenshot](screenshots/image11a.png)

---

#### Screenshot 2 — Output of `free -h`

[Assignment Screenshot](screenshots/image11a.png)

---

#### Screenshot 3 — Output of `df -h`

[Assignment Screenshot](screenshots/image11b.png)

---

#### Screenshot 4 — Output of `sudo du -sh /var/* | sort -h`

[Assignment Screenshot](screenshots/image11b.png)

---

### Notes

Answer the following in your own words:

**1. Which resource looks most critical right now? (CPU/load, memory, or disk) Explain why.**

Judging from the image above, the disk looks most risky right now because /dev/root is 50% full and /var/lib alone is using 532 MB on a small 6.8 GB root volume.

---

**2. What happens if disk becomes 100% full in a production server?**

If the disk fills up completely, production services can fail, Nginx may stop serving files, and the system could become unstable since it cannot write logs, temporary files, or database data.

---

# Task 5 — Configuration & Deployment Verification

## Goal

Ensure the correct React build is deployed and Nginx is serving it properly.

### Evidence

#### Screenshot 1 — Output of `ls -lah /var/www/html | head -n 20`

[Assignment Screenshot](screenshots/image12a.png)

---

#### Screenshot 2 — Output of `grep -R "Deployed by" -n /var/www/html 2>/dev/null | head`

[Assignment Screenshot](screenshots/image12a.png)

---

#### Screenshot 3 — Output of `grep -n "try_files" /etc/nginx/sites-available/default`

[Assignment Screenshot](screenshots/image12a.png)

---

### Notes

Answer the following in your own words:

**1. How do you confirm that the correct version of the application is deployed?**

You can confirm the correct version is deployed by checking for a version label or “Deployed by” note in the website files, like  we did in screenshot no.2 check,  or by visiting the app and verifying key features or version-specific changes.

---

# Task 6 — Nginx Configuration Failure Simulation

## Goal

Simulate a real-world Nginx misconfiguration and recover the service safely.

### Evidence

#### Screenshot 1 — Output of `sudo nginx -t` showing the syntax error (broken config)

[Assignment Screenshot](screenshots/image12b.png)

---

#### Screenshot 2 — Output of `sudo nginx -t` showing syntax ok (fixed config)

[Assignment Screenshot](screenshots/image12b.png)

---

#### Screenshot 3 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

[Assignment Screenshot](screenshots/image12b.png)

---

### Notes

Answer the following in your own words:

**1. What caused the configuration failure?**

I introduced a tiny error in the config by removing the letter ‘y’

---

**2. How did you fix the issue?**

I nano'd back in added the letter back, and saved the configuration.

**3. How can you avoid this kind of issue in real production systems?**

I can avoid this in real prod systems by ensuring each letter, semicolon,etc needed in the config code is complete/ none is missing to prevent broken/failed deployment.

---

# Task 7 — Web Application Failure Simulation

## Goal

Simulate missing deployment content and recover the application safely.

### Evidence

#### Screenshot 1 — Output of `curl -I http://<public-ip>` showing failure (non-200 response)

[Assignment Screenshot](screenshots/image12b.png)

---

#### Screenshot 2 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

[Assignment Screenshot](screenshots/image12b.png)

---

### Notes

Answer the following in your own words:

**1. What caused the application to break in this scenario?**

I introduced a tiny error in the config by removing the letter ‘y’

---

**2. How did you fix the issue and restore the application?**

I nano'd back in added the letter back, and saved the configuration.

---

**3. What steps would you take to prevent this kind of issue in real production systems?**

I can avoid this in real prod systems by ensuring each letter, semicolon,etc needed in the config code is complete/ none is missing to prevent broken/failed deployment.

---

# Task 8 — Security & Reliability Review

## Goal

Review and reflect on the security and reliability practices applied during this assignment.

### Security & Reliability Notes

Answer the following in your own words:

**1. Why is SSH key-based authentication more secure than sharing passwords?**

SSH keys are harder to guess or steal than passwords and provide more secure access.

---

**2. Why should only required ports be open on a production server?**

To reduce security risks and prevent unauthorized access to certain services.

---

**3. Why is it important for Nginx to be enabled on boot?**

So Nginx starts automatically when the server restarts, keeping the website available.

---

**4. What are the risks of sharing secrets, keys, or credentials publicly?**

Others could use them to access your systems, data, or cloud resources, which could lead to security breaches or financial loss.

---

**5. Why should cloud resources be stopped or terminated when they are no longer needed?**

To avoid unnecessary costs and reduce the number of resources that could become a security risk.


---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Do not expose sensitive information (keys, passwords, account IDs)

---

# Completion Checklist

- [ ] Task 1: Screenshots (browser, ip a, ss -tulpen, ufw status) + Notes answered
- [ ] Task 2: Screenshots (nginx status, nginx -t, ss port 80) + Notes answered
- [ ] Task 3: Screenshots (access log, error log, journalctl) + Notes answered
- [ ] Task 4: Screenshots (uptime, free -h, df -h, du -sh) + Notes answered
- [ ] Task 5: Screenshots (ls html, grep deployed by, grep try_files) + Notes answered
- [ ] Task 6: Screenshots (nginx -t fail, nginx -t pass, curl recovery) + Notes answered
- [ ] Task 7: Screenshots (curl failure, curl recovery) + Notes answered
- [ ] Task 8: Security & Reliability Notes answered
- [ ] LinkedIn post published and URL submitted
- [ ] Full Name visible in all required screenshots
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