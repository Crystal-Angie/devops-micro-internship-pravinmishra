# Assignment 6 — Build an AI-Assisted Linux Health Check (AI-Assisted Linux Incident Triage)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash triage script that checks the health of your Ubuntu server and Nginx application, connect it to Claude Code as a reusable `/linux-triage` skill, simulate a controlled Nginx incident, use the skill to gather and analyze evidence, recover the service manually, and verify recovery. The workflow follows the Agentic Loop: Gather → Analyze → Human Act → Verify.

---

# Task 1 — Confirm the Healthy Baseline and Create the Workspace

## Goal

Confirm that Nginx and the React application are healthy before building the automation.

### Evidence

#### Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhost`

[Assignment screenshot](screenshots/image25a.png)

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure

[Assignment screenshot](screenshots/image25b.png)
---

### Notes

Answer the following in your own words:

**1. What proves that Nginx is running?**

The systemctl is-active nginx command returns active, showing that the Nginx service is running.

---

**2. What proves that the server is listening for HTTP traffic?**

The ss -ltn | grep ':80' command shows that port 80 is listening for incoming HTTP connections.

---

**3. Why must you capture a healthy baseline before simulating an incident?**

It gives us a known healthy state to compare against after the incident is introduced.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Tell Claude exactly what this project does and what it is not allowed to do.

### Evidence

#### Screenshot 3 — CLAUDE.md open in VS Code showing all four sections (Project Overview, Incident Workflow, Safety Rules, Output Rules)

[Assignment screenshot](screenshots/image25c.png)

---

### Notes

Answer the following in your own words:

**1. Why should Claude receive project-specific operational rules?**

They tell Claude how to operate safely within the project and prevent it from performing unauthorized actions.

---

**2. Why is the human required to execute the recovery command?**

Recovery actions can affect the server, so the human should review and approve the action before executing it.

---

**3. Which rule prevents Claude from making an unsupported diagnosis?**

“Do not claim a root cause unless the report contains supporting evidence.”

---

# Task 3 — Use Agentic AI to Plan Before Writing the Script

## Goal

Use Claude Code to inspect the environment and produce a read-only plan before creating any Bash code.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results

[Assignment screenshot](screenshots/image26a.png)
[Assignment screenshot](screenshots/image26b.png)
[Assignment screenshot](screenshots/image26c.png)

---

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

Claude inspecting the server using read-only commands represents the Gather phase.

---

**2. Did Claude follow the instruction not to create files? How did you verify this?**

Yes. I verified that Claude only performed read-only inspection and did not create or modify project files by going through and inspecting the project files again, only the previous existing files were present.
Also, in the first screenshot above, claude clearly states it 'read CLAUDE.md.', it doesn't mention anything abouting file creation or even prompt it after  proposing the five checks Bash incident-triage plan.

---

**3. Why is planning before coding useful in DevOps automation?**

It helps define the required checks and logic before writing the automation, thereby reducing mistakes and unnecessary changes.

---

# Task 4 — Build the Linux Triage Bash Script

## Goal

Create one Bash script that gathers consistent Linux and Nginx health evidence.

### Evidence

#### Screenshot 5 — Top section of `linux-triage.sh` showing variables, thresholds, and the checks array

[Assignment screenshot](screenshots/image27a.png)

---

#### Screenshot 6 — Middle section showing check functions and conditionals

[Assignment screenshot](screenshots/image27b.png)

---

#### Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior

[Assignment screenshot](screenshots/image27c.png)

---

#### Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission

[Assignment screenshot](screenshots/image27d.png)

---

### Notes

Answer the following in your own words:

**1. What is stored in the checks array?**

The array contains the five functions to execute:

checks=(
  check_service
  check_port
  check_http
  check_disk
  check_memory
)
---

**2. How does the `for` loop use that array?**

The for loop goes through each function name in the array and runs that health check one by one.:

'for check_function in "${checks[@]}"
do
  "$check_function"
done'

---

**3. Why are the health checks separated into functions?**

Separating the checks into functions makes the script easier to read, organize, test, and maintain. Each function is responsible for one specific check.

---

**4. What is the purpose of `$(...)` in this script?**

$(...) is used for command substitution. It runs a command and puts its output into a variable or another part of the script, such as getting the hostname or disk usage.
---

**5. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

Different exit codes allow other commands or systems to quickly understand the result. 
 0 means healthy
 1 means there is a warning and 
 2 means the health check failed.

---

# Task 5 — Run and Understand the Healthy-State Report

## Goal

Run the Bash script against the healthy server and verify that it creates a report.

### Evidence

#### Screenshot 9 — Output of `./scripts/linux-triage.sh` showing your Full Name and all five check results

[Assignment screenshot](screenshots/image28a.png)

---

#### Screenshot 10 — Output showing the captured exit code and final summary

[Assignment screenshot](screenshots/image28aa.png)
[Assignment screenshot](screenshots/image28b.png)
[Assignment screenshot](screenshots/image28c.png)

---

### Notes

Answer the following in your own words:

**1. What is the overall status of your healthy baseline?**

The overall status is WARN because all one of my health checks(Root disk usage) is at its warning threshold.

---

**2. Which exact Linux evidence proves the application is serving traffic?**

The curl -I http://localhost command returning HTTP 200 OK proves that the application is responding to HTTP requests. The listening state on port 80 also confirms that a service is accepting HTTP traffic.

---

**3. Did your script return exit code 0 or 1? Explain why.**

## My script returned exit code 1 = WARN , this is because one of my five health checks is at the WARN threshold. Exit code 1 is used when there is at least one warning but no failures.

---

**4. What is the difference between a warning and a failure in this script?**

A warning means the system has a condition that may need attention but is still considered operational, such as memory below the warning threshold, disk usage in my case. A failure means an important health check has failed, such as Nginx being inactive, port 80 not listening, disk usage almost filled up, unlike a warning, FAIL means the server is not operational under such conditions.

---

# Task 6 — Create and Run the /linux-triage Skill

## Goal

Turn the Bash script into a reusable, manually invoked Agentic AI workflow.

### Evidence

#### Screenshot 11 — `SKILL.md` showing the frontmatter, allowed tool restrictions, and safety rules

[Assignment screenshot](screenshots/image29a.png)

---

#### Screenshot 12 — `/linux-triage` output for the healthy server

[Assignment screenshot](screenshots/image29b.png)
[Assignment screenshot](screenshots/image29c.png)

---

### Notes

Answer the following in your own words:

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

The skill is designed to inspect and analyze the server without changing files. Bash collects system information, while Read and Grep allow Claude to read and examine the evidence. Write is not included because the skill should not modify files.

---

**2. Why is `disable-model-invocation: true` useful for this skill?**

It prevents Claude from automatically deciding to run the skill on its own. The skill must be manually invoked by the human, giving the engineer control over when the health check runs.

---

**3. What part is performed by Bash, and what part is performed by Claude?**

Bash collects the evidence by checking Nginx, port 80, HTTP response, disk usage, and memory. Claude analyzes that evidence, identifies warnings or failures, and recommends a safe next step.

---

**4. Why is this better than asking Claude "Is my server healthy?" without giving it evidence?**

The health-check script provides actual, current evidence from the server instead of relying on assumptions. This makes Claude's analysis more accurate and helps prevent unsupported conclusions about the cause of a problem.

---

# Task 7 — Simulate an Nginx Incident and Let the Skill Diagnose It

## Goal

Create a controlled service failure, gather evidence through Bash, and let Claude analyze the evidence without taking recovery action.

### Evidence

#### Screenshot 13 — Output showing Nginx is inactive and the HTTP request fails

[Assignment screenshot](screenshots/image30a.png)

---

#### Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command

[Assignment screenshot](screenshots/image30b.png)
[Assignment screenshot](screenshots/image30c.png)

---

#### Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name

[Assignment screenshot](screenshots/image30d.png)

---

### Notes

Answer the following in your own words:

**1. Which three checks failed?**

The three failed checks were:

Nginx service is not active
Port 80 is not listening
Local HTTP check failed

---

**2. What evidence supports the conclusion that Nginx is unavailable?**

The Bash report shows that Nginx is inactive, port 80 is not listening, and the localhost HTTP request does not return a successful response. Together, this evidence shows that Nginx is unavailable.

---

**3. Did Claude execute the recovery command? Why is that important?**

No. Claude only recommended a recovery command for the human to review and execute. This is important because the workflow requires human approval before any recovery action, preventing the AI from making potentially unsafe changes automatically.

---

**4. Which phase of the Agentic Loop is represented by the Bash report?**

The Bash report represents the Gather Evidence phase because the script collects facts about the server's current condition.

---

**5. Which phase is represented by Claude's explanation?**

Claude's explanation represents the Analyze Evidence phase because Claude interprets the collected evidence, identifies the likely problem, and recommends the next safe action.

---

# Task 8 — Recover Manually, Verify Again, and Write the Incident Summary

## Goal

Recover the service as the human operator and prove that the system is healthy again.

### Evidence

#### Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK

[Assignment screenshot](screenshots/image31a.png)

---

#### Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results

[Assignment screenshot](screenshots/image31b.png)

---

#### Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`

[Assignment screenshot](screenshots/image31c.png)

---

#### Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name

[Assignment screenshot](screenshots/image31d.png)
[Assignment screenshot](screenshots/image31e.png)

---

### Notes

Answer the following in your own words:

**1. What action did you execute manually?**

I manually started the Nginx service using sudo systemctl start nginx.

---

**2. What evidence proves that the service recovered?**

'systemctl is-active nginx' returned active, and curl -I http://localhost returned a successful HTTP 200 OK response.

---

**3. Why is the second triage run necessary?**

The second triage run verifies that the recovery action actually fixed the problem and that the system is healthy again.

---

**4. What could go wrong if an AI agent automatically restarted every failed service?**

It could restart the wrong service, cause downtime, interrupt another application, or make a problem worse without human approval.

---

**5. In one sentence, explain the difference between using AI as a chatbot and using AI in this agentic workflow.**

A chatbot mainly answers questions, while this agentic workflow uses Bash to gather evidence, Claude to analyze it, a human to approve the action, and another check to verify the result.

---

# Incident Summary

Fill in all seven sections below in your own words.

**Full Name:** Angela C. Chibuike

**Date:** 15/09/2026

---

**1. Reported Symptom**

Nginx appeared to have stopped either manually or  because the server was experiencing resource pressure. The triage report showed that Nginx had stopped and needed to recover.

---

**2. Evidence Collected**

The failed Bash checks showed;
[FAIL] Nginx service is not active
[FAIL] Port 80 is not listening
[FAIL] Local HTTP check returned status 000(connection refused)
[WARN] Root disk usage is 89%
The Nginx logs also showed that the service stopped at 15:47:08

---

**3. Most Likely Cause**

From the collected evidence, the most likely cause was low memory arising from the limited available disk space and memory.

Note: While low memory was inferred as the likely cause given the limited resources available arising from the AWS t3.medium instance
in use - running React application, dependencies, Claude, and other system processes-The Nginx outage was caused by the manual
stopping of the Nginx service for incident simulation.
Thus, it is advisable to use t3.large when carrying out this project for more accurate analysis.
This further demonstrates why human oversight matters in agentic systems.And also, the importance of instance sizing as an 
operational consideration.

---

**4. Human-Approved Recovery Action**

The recovery commands were reviewed before being executed manually. The disk and memory status was checked with:

du -sh /* | sort -rh && free -h

The system was then cleaned up to reduce disk usage and improve available resources.
After which, I restarted nginx with;
sudo systemctl restart Nginx

---

**5. Verification**

The recovery was confirmed by the following results:
systemctl is-active nginx = active
curl -I http://localhost = 200 OK

These outputs showed that both Nginx and the web application were accessible again.

---

**6. Safety Decision**

The AI skill was allowed only the read and inspect tools to gather and analyze evidence because these actions were read-only and did not change the server.
It was not allowed to restart Nginx because restarting a service can affect the running application. Thus strictly implementing actions requiring human review and manual approval served as safety nets in this project.

---

**7. Agentic Loop Mapping**

The incident followed the agentic loop:

Gather → Analyze → Human Act → Verify

Gather: The skill collected Nginx, disk, memory, HTTP, and log information.
Analyze: It identified that Nginx had been stopped and that the server had limited available resources.
Human Act: The recovery action was reviewed and performed manually.
Verify: The checks confirmed that Nginx was active, port 80 was listening, and the application returned HTTP 200.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/angela-chibuike_devops-cloudcomputing-linux-activity-7420345062426992641-U8MD?utm_source=share&utm_medium=member_desktop&rcm=ACoAADn-PSABhIre4cnftTYXk433XaYMG-l_k9Y `

---

#### Screenshot — Published LinkedIn post

[Assignment screenshot](screenshots/image32a.png)

---

# GitHub Repository URL

Paste the URL of your GitHub folder or repository containing the assignment files here:

`https://github.com/Crystal-Angie/my-react-app-week-03-agentic-linux.git `

---

# Submission Instructions

- Add all required screenshots in your submission
- Full Name must be visible in required screenshots and the Bash report
- All written answers must be in your own words
- Do not expose sensitive information (keys, passwords, AWS account IDs, tokens)
- GitHub URL must be included in this document

---

# Completion Checklist

- [ ] Task 1: Healthy baseline confirmed, workspace created (Screenshots 1–2, Notes answered)
- [ ] Task 2: CLAUDE.md created with all four sections (Screenshot 3, Notes answered)
- [ ] Task 3: Five-check plan produced by Claude using read-only tools (Screenshot 4, Notes answered)
- [ ] Task 4: `linux-triage.sh` created, syntax validated, executable permission set (Screenshots 5–8, Notes answered)
- [ ] Task 5: Healthy-state report generated with no FAIL result (Screenshots 9–10, Notes answered)
- [ ] Task 6: `/linux-triage` skill created and run successfully on healthy server (Screenshots 11–12, Notes answered)
- [ ] Task 7: Nginx incident simulated, failed evidence captured, Claude did not execute recovery (Screenshots 13–15, Notes answered)
- [ ] Task 8: Nginx recovered manually, recovery verified, reports saved, incident summary complete (Screenshots 16–19, Notes answered)
- [ ] Incident summary contains all seven required sections
- [ ] LinkedIn post published and URL submitted
- [ ] Full Name visible in all required screenshots and the Bash report
- [ ] Skill does not have Write permission
- [ ] Skill did not execute any recovery commands
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