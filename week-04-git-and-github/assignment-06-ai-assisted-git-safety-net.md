# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In Week 2 you built Claude Code hooks that block a dangerous action *before* it happens (`PreToolUse`), and a restricted skill that could look but not touch (`allowed-tools` without `Write`). In this assignment you will discover that Git has the exact same idea, decades older: a **pre-commit hook** that blocks a commit before it's created.

You will build both halves of a real "PR Ready" workflow:

1. A **Git hook that follows fixed rules** — scans staged changes for hardcoded secrets and oversized files and refuses the commit. No AI involved, no guessing, just a rule that gives the same answer every time.
2. A **restricted Claude Code skill** (`/pr-ready`) that reads your staged diff and drafts a Pull Request title, description, and a short list of things worth a second look — the kind of judgment a fixed rule can't make (mixed changes, missing context, unclear intent). The skill never commits, pushes, or opens the PR. You do that yourself, using its draft as a starting point.

This mirrors the Agentic Loop from Week 3's Linux triage assignment: **Gather → Analyze → Human Act → Verify**. The hook and the skill both gather and analyze; only you act.

---

# Task 0 — Confirm Your Fork and Create a Feature Branch

## Goal

Confirm you are working in your own fork, then create a dedicated branch for this assignment.

### Evidence

#### Screenshot 1 — Output of git remote -v and git branch showing the new branch

[Assignment screenshot](screenshots/week4-06a.png)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

A dedicated branch keeps my assignment work separate from the main branch. This makes it safer to make changes, test them, and review them before merging into main.

---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

[Assignment screenshot](screenshots/week4-06b.png)

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

The assignment uses a fake key to safely test the secret-detection hook without exposing a real API key or password. This lets us see how the hook works without risking sensitive information. Also, in real case scenario, this is not allowed, so to test against mistakes like commiting sensitive info, this assignment uses a fake key.

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

[Assignment screenshot](screenshots/week4-06c.png)

---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

[Assignment screenshot](screenshots/week4-06d.png)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

Because putting it in the repo means everyone who clones the project can get the same hook. It makes the safety rules easy to share and keep consistent.

---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

PreToolUse checks an AI tool action before it runs, while the Git pre-commit hook checks a Git commit before it is created. Both can stop an action before it happens if it breaks a defined rule.

---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

[Assigment screenshot](screenshots/week4-06e.png)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

The line that matched the fake key was:

if git diff --cached -- "$file" | grep -qE 'AKIA[0-9A-Z]{16}|-----BEGIN (RSA|OPENSSH|PRIVATE) KEY-----'; then

It matched because the grep -qE pattern specifically looks for an AWS Access Key ID beginning with AKIA followed by 16 uppercase letters or numbers. My fake key matched this pattern, so the hook detected it as a possible secret and blocked the commit.

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

No. The hook would not catch a secret stored in a variable if its value did not match one of the patterns in the rule, such as the AKIA prefix or a private-key header. This shows that fixed rules can only detect the specific patterns they were designed to look for, so they may miss other types of secrets.
---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

[Assignment screenshot](screenshots/week4-06f.png)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

[Assignment screenshot](screenshots/week4-06g.png)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

/pr-ready has Bash and Read because it needs to check the staged changes and read files. It does not have Write because it should only review and report problems, not change any files.

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

No, they did not flag exactly the same things. The pre-commit hook only caught the fake secret key through detecting already predefined secret patterns,
while /pr-ready went further to flag both the secret key ,the debug echo statement, and lack of clear file purpose/documentation. This shows that AI using /pr-ready can perform a broader review, while the pre-commit hook mainly checks for specific secret patterns and oversized files.

---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

[Assignment screenshot](screenshots/week4-06h.png)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

[Assignment screenshots](screenshots/week4-06i.png)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

I changed 3 things;

- I removed the fake AWS credential
Deleted:
AWS_ACCESS_KEY_ID=AKIAABCDEFGHIJKLMNOP
This removed the AKIA... pattern that the pre-commit hook was detecting as a possible secret.

- Removed the debug statement that exposed the credential
Deleted:
echo "DEBUG: token is $AWS_ACCESS_KEY_ID"
Replaced it with:
echo "Notification script running"
This prevents a credential from being printed to the terminal/logs.

- I also updated the comment from saying it contained a fake credential to saying it is a placeholder notification test with no sensitive credentials.

---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

[Assignment screenshot](screenshots/week4-06j.png)
[Assignment screenshot](screenshots/week4-06k.png)

---

#### PR Link

(https://github.com/Crystal-Angie/devops-micro-internship-pravinmishra/pull/1/changes/28c383def80efb8eb454908066d168ebae469bcf)

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I didn't add because the AI's drafted PR description was mostly accurate of the change made, I removed few words because they sounded repetitive and also tried to keep the description simple.

---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

The AI could include incorrect or missing information about the changes. This could make the Pull Request description misleading and cause reviewers to misunderstand what was actually changed.

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

This PR targets my own fork because the scripts and skills are practice work, not meant for the shared class repo, so targeting my copy of the repository where I have permission to push and manage branches lets me test and review my changes without directly modifying the shared upstream repository.

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

Gather is when the pre-commit hook and /pr-ready check the staged files and git diff --cached to collect information about the changes.

---

**2. Which step(s) represent Analyze?**

Analyze is when /pr-ready reviews the staged changes for secrets, debug statements, TODOs, unrelated changes, and missing notes, then creates a PR draft.

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

Human Act is when I review the AI's output, make any necessary edits, commit the changes, push the branch, and open the Pull Request. A human should do these actions because they are actions that make changes to the external or shared repo and also determine what changes are published and submitted for review. So a human with discerning judgement should be the one to carry this out.

---

**4. Which step is Verify?**

Verify is checking the final Pull Request to make sure it has the correct base repository, title, description, and changes.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

The pre-commit hook provides a fixed and predictable check for known patterns such as secrets. The AI skill can review the changes more broadly and identify issues that the fixed rules may miss, so they complement each other.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

https://www.linkedin.com/posts/angela-chibuike_dmibypravinmishra-git-github-ugcPost-7508113626654986241-wkUG/?utm_source=share&utm_medium=member_desktop&rcm=ACoAADn-PSABhIre4cnftTYXk433XaYMG-l_k9Y 

---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

* I Learned how to create a **pre-commit hook** and how it can detect possible secrets and oversized files before a commit.
* Learned how to create and use a **Claude Code skill** to review staged changes without modifying files.
* Learned why **fixed rules(git pre-commit  hook) and AI review(skills)** can complement each other when checking code.
* Learned how to safely **push changes to my own fork and open a Pull Request** for review.
* I learned that fixed rules can detect known secret patterns but may miss secrets that use different formats.
* I learned how a Git pre-commit hook is similar to `PreToolUse` because both can stop an action before it happens.
* I learned how to use Claude Code to review staged changes and prepare a PR without allowing it to commit or push changes.

---

# Submission Instructions

- Ensure `hooks/pre-commit` and `.claude/skills/pr-ready/SKILL.md` are committed to your GitHub repository
- Add all required screenshots to your submission
- All written answers must be in your own words
- Do not use a real secret or credential anywhere in your submission — the fake key in Task 1 is intentional and must stay clearly fake
- Open your Pull Request against your own fork, not the shared upstream repository
- Push your final changes to your forked repository
- Include your PR link and LinkedIn post URL

---

## GitHub Repository URL

Paste your forked repository URL here:

`https://github.com/Crystal-Angie/devops-micro-internship-pravinmishra.git `

---

# Completion Checklist

- [ ] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
- [ ] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
- [ ] `core.hooksPath` configured to point at `hooks/`
- [ ] Pre-commit hook shown blocking the risky commit
- [ ] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
- [ ] `/pr-ready` run against the risky diff and shown flagging issues
- [ ] Risky file fixed; `git commit` succeeds cleanly
- [ ] `/pr-ready` re-run showing a clean report and drafted PR title/description
- [ ] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
- [ ] Agentic Loop mapping (Task 7) completed in your own words
- [ ] LinkedIn post published and URL submitted
- [ ] All required screenshots added
- [ ] GitHub repository URL provided

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
