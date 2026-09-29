# Assignment 5 — AI-Assisted Sprint Health Report via Jira MCP

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will connect Claude Code to your Jira board through an MCP server, the same way you connected it to GitHub in Week 2, and build a read-only `/sprint-health` skill. The skill reads your current sprint through Jira's API and reports sprint velocity, stories at risk of missing the sprint, and items missing an estimate — but it must never create, edit, comment on, or transition a single ticket itself. You will prove that boundary holds by making a real change on the board yourself and confirming the skill only ever reports, never acts.

---

# Task 1 — Create a Jira API Token

## Goal

Generate an API token from your Atlassian account that the MCP server will use to authenticate with your Jira site. Do not screenshot the token value itself.

### Evidence

#### Screenshot 1 — Jira API token creation confirmation page showing the token name, with the token value not visible

[Assignment screenshot](screenshots/jira%20api-01.png)
[Assignment screenshot](screenshots/week5-5a.png)
[Assignment screenshot](screenshots/week5-5b.png)

### Notes You Must Write (Very Important):

Why does the MCP server need your site URL and account email in addition to the token?

The MCP server needs my Jira site URL, account email, and API token because each work together to enable claude authenticate to Jira's API and carry out intended tasks.

The site URL identifies the Jira site to connect to, the email identifies the user/account associated with the API token, and the API token verifies authentication and allows the MCP server to make API requests. Together, they allow the MCP server to connect to the correct Jira site using my account's credentials and permissions.

---

# Task 2 — Create .mcp.json at the Project Root

## Goal

Create or update `.mcp.json` at your project root with a Jira MCP server block, following the same shape as the GitHub MCP server you configured in Week 2.

### Evidence

#### Screenshot 2 — `.mcp.json` open in VS Code showing the Jira server configuration

[Assignment screenshot](screenshots/week5-5c.png)

### Notes You Must Write (Very Important):

Compare this jira block to the github block from Week 2 Assignment 5. The GitHub server ran via npx (a Node.js package); this one runs via uvx (a Python package) — what stays exactly the same shape despite that difference, and why doesn't Claude Code care which language a given MCP server is written in?

The Jira and GitHub MCP blocks have the same basic structure: both define an MCP server name, a command used to launch the server, arguments passed to that command, and an environment section. The main difference is that GitHub used npx, which runs a Node.js package, while Jira uses uvx, which runs a Python package. 
Claude Code does not need to know what programming language the MCP server was written in because MCP provides a standardized communication interface; Claude only needs to know how to launch the server and communicate with it through that interface.

---

# Task 3 — Add Your Credentials to settings.local.json

## Goal

Add your Jira site URL, account email, and API token to `.claude/settings.local.json`, and confirm that file is listed in `.gitignore` so it is never committed.

### Evidence

#### Screenshot 3 — `settings.local.json` open in VS Code showing the `env` section, with the actual token value blurred or covered

[Assignment screenshot](screenshots/week5-5d.png)

### Notes You Must Write (Very Important):

Why must JIRA_API_TOKEN live in settings.local.json and never in .mcp.json?

JIRA_API_TOKEN is a secret credential that authenticates my personal Atlassian account. The .mcp.json file defines how Claude Code starts the MCP server and can be safely shared or committed. Thus putting the token in .mcp.json could accidentally expose it through Git commits, GitHub repositories, screenshots, or other shared project files. So it must be stored in settings.local.json, which is intended for local, sensitive configuration and is excluded from Git. 

---

# Task 4 — Verify the Connection with /mcp

## Goal

Restart Claude Code and confirm the Jira MCP server shows as connected.

### Evidence

#### Screenshot 4 — `/mcp` output showing `jira: connected`

[Assignment screenshot](screenshots/week5-5e.png)

---

# Task 5 — Run a Live Query to Prove Real Board Data

## Goal

Ask Claude to list the issues in your current active sprint through the Jira MCP connection, and confirm the result matches what you see on your live board in the browser.

### Evidence

#### Screenshot 5 — Claude's response showing the live sprint issue list retrieved via Jira MCP

[Assignment screenshot](screenshots/week5-5f.png)
[Assignment screenshot](screenshots/week5-5g.png)
[Assignemnt screenshot](screenshots/week5-5h.png)

### Notes You Must Write (Very Important):

How did you confirm this was real board data and not something Claude guessed?

I watched it as it called MCP first, then Jira and proceeded to access and analyze my project. Also, all the output matches what I have in the Jira Project.

---

# Task 6 — Build the /sprint-health Skill

## Goal

Create a `/sprint-health` skill restricted to read-only Jira tools plus `Read`, with no issue-mutating tools and no `Write`. Run it and confirm it produces a report covering sprint velocity, at-risk stories, and items missing an estimate.

### Evidence

#### Screenshot 6 — `SKILL.md` frontmatter showing `allowed-tools` limited to read-only Jira tools plus `Read`, with `disable-model-invocation: true`

[Assignment screenshot](screenshots/week5-5i.png)

#### Screenshot 7 — `/sprint-health` output showing the full triage report against your real sprint

[Assignment screenshot](screenshots/week5-5j.png)
[Assignment screenshot](screenshots/week5-5k.png)
[Assignment screenshot](screenshots/week5-5l.png)

### Notes You Must Write (Very Important):

1. Which Jira MCP tools does this skill's allowed-tools list include, and which mutating tools (create issue, update issue, transition issue, add comment) does it deliberately exclude?

The skill uses read-only Jira MCP tools to retrieve sprint details, issues, estimates, and statuses, along with the Read tool. It excludes mutating tools such as create issue, update issue, transition issue, and add comment to prevent changes to the Jira board.

2. Why does a Scrum Master need this restriction more than almost any other role in this course?

A Scrum Master needs accurate sprint data to monitor progress and identify risks. Restricting the skill to read-only tools prevents accidental changes to tickets, estimates, or statuses and ensures the Scrum Master remains in control of board updates.

---

# Task 7 — Prove the Skill Never Mutates the Board

## Goal

Manually update one ticket on your board in the browser (for example, move a story to "Done" or add a missing estimate), then run `/sprint-health` again and confirm the new report reflects your change — proving the skill only ever reads live state and never wrote to the board itself.

### Evidence

#### Screenshot 8 — Second `/sprint-health` run showing the report now reflects your manual board change

[Assignment screenshot](screenshots/week5-5m.png)
[Assignment screenshot](screenshots/week5-5n.png)

Added in a new story from backlog to the sprint with no acceptance criteria and story point estimate, also moved one story in sprint to done.
Both of which are reflected in the second triage run, proving the skill reads live state — while the skill itself performed no write action on Jira at any point.

### Notes You Must Write (Very Important):

Map this assignment to Gather → Analyze → Human Act → Verify from Week 3 Assignment 6. Which step did you perform manually in the browser, and why must that step stay human?

Gather → Analyze → Human Act → Verify
* Gather: The skill retrieved live Jira sprint data, including story statuses, estimates, and progress.

* Analyze: /sprint-health generated a report identifying sprint velocity, at-risk stories, and missing estimates.

* Human Act: I manually added a new story to the sprint without acceptance criteria or a story point estimate and moved another story to Done in Jira.

* Verify: I ran /sprint-health again and confirmed that the report reflected both changes, proving the skill reads live data without modifying the Jira board.

I performed the Human Act step manually by adding a new story to the sprint without acceptance criteria or a story point estimate and moving another story to Done. This step must stay human to ensure that changes to the Jira board are intentional, reviewed, and approved rather than made automatically by the AI.
---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:
- All 8 required screenshots
- All the required notes

---

# Completion Checklist

- [ ] Task 1: Jira API token created, value never screenshotted (Screenshot 1)
- [ ] Task 2: `.mcp.json` has the Jira server block (Screenshot 2)
- [ ] Task 3: Credentials stored in `settings.local.json`, token blurred, file gitignored (Screenshot 3)
- [ ] Task 4: `/mcp` shows the Jira server connected (Screenshot 4)
- [ ] Task 5: Live query returned real sprint data, verified against the browser (Screenshot 5)
- [ ] Task 6: `/sprint-health` skill created with correct read-only `allowed-tools`, and produced a full report (Screenshots 6–7)
- [ ] Task 7: A manual board change was reflected in a second `/sprint-health` run (Screenshot 8)
- [ ] Skill never created, edited, transitioned, or commented on any issue
- [ ] Reflection answered (Notes)
- [ ] No API token value exposed

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
