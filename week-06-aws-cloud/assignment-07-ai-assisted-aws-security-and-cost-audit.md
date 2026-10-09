# Assignment 7 — AI-Assisted AWS Security and Cost Audit

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash script that audits the AWS resources you deployed earlier this week — your S3 static site, EC2 instance(s), security groups, RDS database, and EBS volumes — for common security and cost misconfigurations.

You will then connect that script to Claude Code as a reusable `/aws-audit` skill that explains what it found and recommends a fix, without ever making the fix itself.

Finally, you will find a real misconfiguration in your own account, apply the fix yourself, and prove it worked with a second audit run.

---

# Task 1 — Confirm Your AWS Resources and Set Up Your Workspace

## Goal

Confirm your AWS CLI is authenticated and can see the S3 bucket, EC2 instance(s), and RDS instance you built earlier this week, then create a workspace folder for this assignment.

### Evidence

#### Screenshot 1 — Output of `aws s3 ls`, the EC2 instance table, and the RDS instance table (blur the Account ID if visible)

[Assignment screenshot](screenshots/week6-7a.png)

Note: My ec2 and RDS shows no output because there are currently no instances and RDS database running, I shut it down after the assignments to limit costs.
---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort`

[Assignment screenshot](screenshots/week6-7b.png)

---

### Notes You Must Write (Very Important)

**1. Which resources from this week's earlier assignments did you see in the listings?**

The personal portfolio website odeployed on s3 bucket from assignment 2, the EC2 instances and RDS query came back empty because both has been deleted to limit costs

**2. Why must you confirm your resources exist before writing an audit script against them?**

To confirm the resources exist and that the script will audit the correct AWS account and resources.

---

# Task 2 — Define Safety Rules in CLAUDE.md

## Goal

Create a `CLAUDE.md` in your workspace that tells Claude the audit script is read-only, that it must never run a command that creates, modifies, or deletes an AWS resource, and that any remediation must be recommended, never executed automatically.

### Evidence

#### Screenshot 3 — `CLAUDE.md` open in VS Code showing all four sections

[Assignment screenshot](screenshots/week6-7c.png)

---

### Notes You Must Write (Very Important)

**1. Why should Claude never be given permission to run `revoke-security-group-ingress` itself, even if the fix is obviously correct?**

If it changes security group rules, it could interrupt access to an EC2 instance. A human must review and approve the change first.

**2. Which rule prevents Claude from claiming a finding that the report does not support?**

Safety rule No.6 - The rule that says Claude must not claim a finding unless the report contains supporting evidence.

---

# Task 3 — Plan the Audit with Claude Code

## Goal

Ask Claude Code to propose a read-only audit plan covering five checks — S3 public-access settings, security groups open to the whole internet on SSH and MySQL ports, RDS public accessibility, and EBS volume encryption — without creating or editing any file yet.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan

[Assignment screenshot](screenshots/week6-7d.png)
[Assignment screenshot](screenshots/week6-7e.png)
[Assignment screenshot](screenshots/week6-7f.png)
[Assignment screenshot](screenshots/week6-7g.png)

---

### Notes You Must Write (Very Important)

**1. Which part of this task represents the Gather phase?**

The Gather phase is running the five read-only AWS CLI checks to collect evidence about S3 public-access settings, EC2 security groups on ports 22 and 3306, RDS public accessibility, and EBS encryption.

**2. Did every proposed command start with `describe-`, `get-`, or `list-`? Why does that matter?**

Yes, the commands use read-only operations such as describe-, get-, and list-. This matters because they collect information about AWS resources without changing them, helping us identify security risks before recommending any fixes.

---

# Task 4 — Build the AWS Audit Script

## Goal

Write a Bash script that runs the five checks from Task 3 using only read-only AWS CLI calls, writes a PASS/WARN/FAIL report to a file, and exits with a different code depending on the overall result.

Make it executable and confirm it has no syntax errors.

### Evidence

#### Screenshot 5 — Top section of `aws-audit.sh` showing the variables and the checks array

[Assignment screenshot](screenshots/week6-7h.png)

---

#### Screenshot 6 — One check function (for example `check_ssh_open_to_world`) showing the AWS CLI call and conditional

[Assignment screenshot](screenshots/week6-7i.png)

---

#### Screenshot 7 — Output of `bash -n scripts/aws-audit.sh` and `ls -l scripts/aws-audit.sh`

[Assignment screenshot](screenshots/week6-7j.png)

---

### Notes You Must Write (Very Important)

**1. What is stored in the checks array, and how does the loop use it?**

It contains the names of the five functions that perform the audit checks. A loop runs each function in sequence.

**2. Why does every AWS CLI call in this script use `--query` and `--output text` instead of parsing raw JSON?**

They extract the specific values needed and make the results easier for Bash to compare than raw JSON.

**3. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

Different exit codes make it easier to distinguish a healthy audit from warnings and failures: 0 means healthy, 1 means warnings, and 2 means failures.

---

# Task 5 — Run the Baseline Audit

## Goal

Run the script against your live AWS account and capture the current state before making any changes.

### Evidence

#### Screenshot 8 — Output of `./scripts/aws-audit.sh` showing your Full Name and all five checks

[Assignment screenshot](screenshots/week6-7k.png)

---

#### Screenshot 9 — Output showing the captured exit code and final summary

[Assignment screenshot](screenshots/week6-7l.png)

---

### Notes You Must Write (Very Important)

**1. What is the overall status of your baseline audit?**

The overall status is FAIL, with 2 PASS checks, 1 WARN, and 2 FAIL checks. The script returned exit code 2.

**2. Did any check return FAIL or WARN? If so, which one, and what evidence did it show?**

Yes. Two checks returned FAIL:

- S3 public-access settings: BlockPublicAcls=False and IgnorePublicAcls=False, meaning the bucket does not fully block public ACLs.
- EC2 security groups: 11 security groups allow SSH access (port 22) from 0.0.0.0/0, meaning anyone on the internet could attempt to connect.

One check returned WARN:
- RDS public accessibility: The script could not determine the public accessibility of RDS instance '' because no instance ID was configured or the instance could not be found.

The other two checks passed: no security group allows MySQL (port 3306) from anywhere, and all EBS volumes are encrypted.

**3. If every check passed, what does that tell you about the security posture of your account so far?**

Not all checks passed in this audit. The findings show that some security risks need attention, particularly the S3 public-access settings and SSH rules open to the internet. These findings should be reviewed before making any changes, and remediation should only be performed after human approval.

---

# Task 6 — Build and Run the /aws-audit Skill

## Goal

Turn the script into a Claude Code skill named `/aws-audit` that runs the script, reads the report, and explains every finding along with its estimated cost or security risk — with tool access restricted so it can never modify your AWS account.

### Evidence

#### Screenshot 10 — `SKILL.md` showing the frontmatter, tool restrictions, and safety rules

[Assignment screenshots](screenshots/week6-7m.png)

---

#### Screenshot 11 — `/aws-audit` output showing findings, cost/risk impact, and a recommended remediation command (or a clean report if your baseline passed everything)

[Assignment screenshot](screenshots/week6-7n.png)
[Assignment screenshot](screenshots/week6-7o.png)
---

### Notes You Must Write (Very Important)

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

Bash runs the audit script, while Read and Grep let Claude inspect the instructions and report. Write is excluded to prevent the skill from editing files.

**2. What part is performed by Bash, and what part is performed by Claude?**

Bash collects/gathers the AWS evidence and records the results. Claude interprets the evidence, explains the risks, and recommends fixes.

**3. Why is estimating cost/risk impact something the AI adds on top of a plain PASS/FAIL script?**

A plain PASS/FAIL script only tells us whether a check succeeded or failed. AI adds value by explaining the potential cost or risk impact of the failure, helping humans prioritize which problems to fix first while also suggesting a suitable next step. For example, it can explain that an exposed S3 bucket may cause a security risk, while an unused AWS resource may lead to unnecessary costs. Human review is still important to verify the findings and approve remediation.

---

# Task 7 — Fix a Real Finding and Re-Verify

## Goal

Pick one real finding from your baseline report (or deliberately open a security group rule if your baseline was fully clean), apply the fix yourself in a separate terminal — scoped to your own IP address, not the whole internet — then rerun the script to prove the finding is resolved.

### Evidence

#### Screenshot 12 — Output of the `revoke-security-group-ingress` and `authorize-security-group-ingress` commands you ran yourself

[Assignment screenshot](screenshots/week6-7p.png)
---

#### Screenshot 13 — Rerun of `./scripts/aws-audit.sh` showing the finding is now PASS

[Assignment screenshot](screenshots/week6-7q.png)

---

### Notes You Must Write (Very Important)

**1. Which exact finding did you fix, and what command did you run?**

- I fixed two findings: 
the S3 bucket's public ACL settings and the security groups allowing SSH from 0.0.0.0/0. 
For S3, I ran aws s3api put-public-access-block to set BlockPublicAcls=true and IgnorePublicAcls=true. 
For SSH, I restricted access to my own public IP using /32. 
The audit now reports PASS for both checks.

**2. Why did you scope the new rule to your own IP address instead of leaving it open to `0.0.0.0/0`?**

Restricting SSH access to my own IP reduces the risk of unauthorized login attempts from anywhere on the internet. The /32 CIDR allows only one IPv4 address(mine), following the principle of least privilege.

**3. Did Claude execute the remediation command, or did you? Why does that matter?**

I executed the remediation commands myself after reviewing Claude's recommendations. This matters because AI can suggest incorrect or risky changes, so human review and approval help prevent accidental security problems.

**4. Which phase of the Agentic Loop does the Bash script represent? Which phase does Claude's explanation represent? Which phase is you running the fix?**

- Gather: The Bash audit script collects AWS security and configuration information.
- Analyze: Claude interprets the findings, explains their potential impact, and recommends remediation.
- Human Act: I review the recommendations and execute the approved remediation commands.
- Verify: I rerun the audit script to confirm that the findings have been resolved.

---

# LinkedIn Post (Required)

## Goal

Create a LinkedIn post including:

- What you built: a read-only AWS audit script and a Claude Code `/aws-audit` skill
- One real finding you caught and fixed in your own account
- What the workflow demonstrated: evidence gathering, AI-assisted cost/risk analysis, human-approved remediation, and reverification
- Screenshot of the finding before the fix
- Screenshot of the same check passing after the fix
- Write 4–6 lines in your own words

Suggested tags:

`#DMIByPravinMishra #AWS #AgenticAI #ClaudeCode #DevOps`

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/feed/update/urn:li:activity:7514337251456110593/ `

---

#### Screenshot of Published LinkedIn Post

[Assignment screenshot](screenshots/week6-7r.png)

---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:

- All 13 required task screenshots
- Answers to every **Notes You Must Write** question
- `CLAUDE.md`
- `scripts/aws-audit.sh`
- `.claude/skills/aws-audit/SKILL.md`
- `reports/aws-audit-report.txt` baseline report and the reverified report from Task 7
- GitHub folder or repository URL containing the assignment files
- Your Full Name visible in the required outputs
- LinkedIn post URL
- Screenshot of the published LinkedIn post
- GitHub repository URL (containing all assignment files)

---

# Completion Checklist

- [ ] Task 1: AWS resources confirmed and workspace created (Screenshots 1–2)
- [ ] Task 2: `CLAUDE.md` created with project context and safety rules (Screenshot 3)
- [ ] Task 3: Claude produced a read-only five-check audit plan before any script existed (Screenshot 4)
- [ ] Task 4: `aws-audit.sh` built, executable, and passes `bash -n` (Screenshots 5–7)
- [ ] Task 5: Baseline audit captured and saved with Full Name visible (Screenshots 8–9)
- [ ] Task 6: `/aws-audit` skill loads and runs successfully with no Write permission (Screenshots 10–11)
- [ ] Task 7: A real finding was fixed by you and reverified as PASS (Screenshots 12–13)
- [ ] Skill never executed a remediation command
- [ ] New security group rule is scoped to your own IP, not `0.0.0.0/0`
- [ ] All 13 required task screenshots are included
- [ ] All "Notes You Must Write" questions are answered in your own words
- [ ] No AWS credentials or unblurred account IDs exposed
- [ ] LinkedIn post published and URL submitted
- [ ] GitHub repository URL included in submission
- [ ] All assignment files committed and visible in GitHub repository

---

# Final Submission

Submit your GitHub repository URL containing all assignment files, screenshots, reports, and output.

### GitHub Repository URL

Paste your GitHub repository URL here:

`https://github.com/Crystal-Angie/devops-micro-internship-pravinmishra.git `

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