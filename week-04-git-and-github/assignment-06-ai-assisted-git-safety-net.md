# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

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

![alt text](<screenshots/Output of git remote v and git branch .png>)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

This allows you to develop, test, and make changes safely without affecting the stable version of the project. Creating a separate branch keeps your work isolated from the main branch. It also makes it easier to create a clean Pull Request that contains only the changes for this assignment.


---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready


![alt text](<screenshots/Output of  git status showing the staged file .png>)
---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

The assignment uses an obviously fake credential to safely simulate a real security risk without exposing an actual AWS credential. This allows reviewers or security tools to practice detecting hardcoded secrets without compromising real infrastructure or accounts.

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

![alt text](<screenshots/hooks pre-commit open in VS Code showing the full script.png>)

---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

![alt text](<screenshots/Output of git config core hooksPath.png>)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

The hook is tracked in the repository so that it can be shared with other contributors and maintained through version control. A hook stored only in .git/hooks/ is local to one developer's machine and is not included when the repository is cloned. By storing it in a tracked hooks/ directory and configuring core.hooksPath, the team can use the same pre-commit security checks consistently.

---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

The Git pre-commit hook intercepts a Git commit before it is created, checking staged changes for risks such as hardcoded secrets or oversized files.

   While

The Claude Code PreToolUse hook intercepts a tool action before Claude executes it, such as blocking a dangerous command like terraform destroy.

Both are preventive safety controls. They inspect an action before it happens, apply predefined rules, and can block the action if a rule is considered unsafe.

---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

![alt text](<screenshots/Terminal showing git commit rejected with the hook's BLOCKED message.png>)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

The grep -qE line matched the fake key because it searches for the pattern AKIA[0-9A-Z]{16}, which matches the AKIA prefix followed by 16 uppercase alphanumeric characters in the fake credential. This caused the pre-commit hook to flag the file as containing a possible secret.

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

No. The hook would likely not catch a secret stored in a poorly named variable if the value does not contain the AKIA prefix or a private-key header.

---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

![alt text](<screenshots/SKILL.md frontmatter showing allowed-tools.png>)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

![alt text](<screenshots/pr-ready output while the risky file is still staged.png>)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

/pr-ready has Bash and Read so it can inspect the repository and analyze staged changes. It does not have Write because the skill should only review and report findings, not modify files. This follows the principle of least privilege and ensures that the final decision to edit, commit, push, or create a PR remains with the human.

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

Both the pre-commit hook and /pr-ready identified the hardcoded-looking AWS credential. However, /pr-ready also flagged the leftover debug statement and provided a more contextual review of the change. The pre-commit hook is limited to fixed rules, while /pr-ready can analyze the staged diff more broadly and identify issues that do not match a predefined pattern. This shows why the two controls complement each other.
---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

![alt text](<screenshots/git commit succeeding after the fix.png>)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

![alt text](<screenshots/Second pr-ready run showing a clean risk report.png>)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

I changed the scripts/notify.sh file to remove the hardcoded AWS access-key-like value and the leftover debug statement. Specifically, I replaced the fake credential with a safe placeholder/reference and removed the echo "DEBUG:..." line. After staging the updated file again, the pre-commit hook no longer detected a secret-like pattern, so the commit was allowed.

---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

![alt text](<screenshots/Your Pull Request showing the base repository .png>)

---

#### PR Link

https://github.com/pravinmishraaws/devops-micro-internship-interviews/pull/462
---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I edited the AI’s drafted PR description to make it more concise and accurate. I removed unnecessary details, clarified the changes I actually made, and added the verification results from my tests. I did this because the AI’s draft is a starting point, but the final PR description should accurately reflect my work and be reviewed by a human before submission.

---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

If I had blindly copied the AI’s draft without reviewing it, it could contain inaccurate information, incorrect claims, or details that do not reflect my actual changes. I could also unintentionally include sensitive information or misunderstand the purpose of the changes. This is why human review is important before using AI-generated content in a Pull Request.

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

This PR needs to target my own fork because the assignment is for practice and demonstration, not a contribution to the shared upstream repository. Using my fork gives me full control over the changes without modifying the original project. It also allows me to safely test the Git safety hook and /pr-ready workflow while following the proper fork-and-pull-request process.

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

Gather is represented by the Git pre-commit hook and the /pr-ready skill reading the staged changes. They gather information from the staged files and Git diff, including the files changed, their contents, and potential risks such as hardcoded secrets, debug statements, and oversized files.

---

**2. Which step(s) represent Analyze?**

Analyze is represented by the pre-commit hook applying fixed rules to detect secret-like patterns and oversized files, and by the /pr-ready skill reviewing the staged diff for issues such as hardcoded secrets, debug statements, mixed changes, and unclear intent. The /pr-ready skill adds contextual analysis that fixed rules may not detect.

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

Human Act is the step where I review the findings from the pre-commit hook and /pr-ready, make any necessary corrections, and then manually run git commit, git push, and open the Pull Request. A human must perform these actions because AI recommendations should be reviewed before making changes that affect the repository or shared codebase. This keeps the final decision and responsibility with the developer and prevents Claude from independently committing, pushing, or creating a PR without human approval.

---

**4. Which step is Verify?**

Verify is the step where I confirm that the safety checks and final changes worked as expected. This includes checking that the pre-commit hook blocks risky commits, confirming the corrected changes pass the hook, reviewing the final Git status and diff, and verifying that the correct branch and Pull Request were pushed successfully.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

You need both because the pre-commit hook provides consistent, automatic protection against known risks, while the AI skill provides broader, contextual analysis that can identify issues fixed rules may miss. Together, they provide stronger and more reliable protection than either one alone.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

https://lnkd.in/p/dW9wstpv

---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

Learned how to implement Git pre-commit hooks to automatically detect security risks before changes are committed.
Strengthened my understanding of the Gather → Analyze → Human Act → Verify Agentic AI workflow.
Improved my understanding of secure Git workflows, Pull Requests, and human oversight in AI-assisted DevOps.


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

https://github.com/Favoured16/devops-micro-internship-interviews.git

---

# Completion Checklist

- [✅] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
- [✅] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
- [✅] `core.hooksPath` configured to point at `hooks/`
- [✅] Pre-commit hook shown blocking the risky commit
- [✅] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
- [✅] `/pr-ready` run against the risky diff and shown flagging issues
- [✅] Risky file fixed; `git commit` succeeds cleanly
- [✅] `/pr-ready` re-run showing a clean report and drafted PR title/description
- [✅] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
- [✅] Agentic Loop mapping (Task 7) completed in your own words
- [✅] LinkedIn post published and URL submitted
- [✅] All required screenshots added
- [✅] GitHub repository URL provided

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

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
