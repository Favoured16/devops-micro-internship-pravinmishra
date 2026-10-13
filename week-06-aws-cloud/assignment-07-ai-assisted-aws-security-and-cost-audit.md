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

![alt text](<screenshots/Output of Aws S3 ls,RDS and EC2 Instance Table.png>)

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort`

![alt text](<screenshots/Output of`pwd and find  maxdepth 4 type d  sort.png>)

---

### Notes You Must Write (Very Important)

**1. Which resources from this week's earlier assignments did you see in the listings?**

I found two resources from the earlier assignments in the AWS listings: the primary RDS database, book-review-db, and its read replica, book-review-db-replica. Both were in the available status.

**2. Why must you confirm your resources exist before writing an audit script against them?**

I must confirm that the resources exist before writing an audit script so that the script targets the correct AWS resources. This prevents errors, false audit results, and checks against resources that are missing or incorrectly named. It also ensures that the audit accurately reflects the current AWS environment.

---

# Task 2 — Define Safety Rules in CLAUDE.md

## Goal

Create a `CLAUDE.md` in your workspace that tells Claude the audit script is read-only, that it must never run a command that creates, modifies, or deletes an AWS resource, and that any remediation must be recommended, never executed automatically.

### Evidence

#### Screenshot 3 — `CLAUDE.md` open in VS Code showing all four sections

![alt text](<screenshots/CLAUDE.md` open in VS Code showing all four sections.png>)

---

### Notes You Must Write (Very Important)

**1. Why should Claude never be given permission to run `revoke-security-group-ingress` itself, even if the fix is obviously correct?**

Claude should not be given permission to run revoke-security-group-ingress itself because it is a destructive change to live AWS security rules. Even when the recommended fix appears correct, an automated action could block legitimate traffic or cause service disruption. Claude should identify and explain the required change, while a human reviews and approves the action before it is executed.

**2. Which rule prevents Claude from claiming a finding that the report does not support?**

The rule that prevents Claude from claiming an unsupported finding is the evidence-based reporting rule. It requires every finding in the report to be supported by actual evidence from the audit results. Claude must not assume, infer, or claim a problem that the collected evidence does not demonstrate.

---

# Task 3 — Plan the Audit with Claude Code

## Goal

Ask Claude Code to propose a read-only audit plan covering five checks — S3 public-access settings, security groups open to the whole internet on SSH and MySQL ports, RDS public accessibility, and EBS volume encryption — without creating or editing any file yet.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan

![alt text](<screenshots/Claude Code showing the five-check plan.png>)

---

### Notes You Must Write (Very Important)

**1. Which part of this task represents the Gather phase?**

The Gather phase is represented by listing and inspecting the existing AWS resources, such as using AWS CLI commands to confirm that the RDS instances and other resources from the earlier assignments exist and to collect their current status and configuration details.

**2. Did every proposed command start with `describe-`, `get-`, or `list-`? Why does that matter?**

Yes. Every proposed command should start with a read-only operation such as describe-, get-, or list-. This matters because these commands inspect AWS resources without changing them. It keeps the audit in a safe, evidence-gathering phase and prevents Claude from accidentally modifying or deleting live infrastructure.

---

# Task 4 — Build the AWS Audit Script

## Goal

Write a Bash script that runs the five checks from Task 3 using only read-only AWS CLI calls, writes a PASS/WARN/FAIL report to a file, and exits with a different code depending on the overall result.

Make it executable and confirm it has no syntax errors.

### Evidence

#### Screenshot 5 — Top section of `aws-audit.sh` showing the variables and the checks array

![alt text](<screenshots/Top section of `aws-audit.sh-2.png>)

---

#### Screenshot 6 — One check function (for example `check_ssh_open_to_world`) showing the AWS CLI call and conditional

![alt text](<screenshots/One check function.png>)

---

#### Screenshot 7 — Output of `bash -n scripts/aws-audit.sh` and `ls -l scripts/aws-audit.sh`

![alt text](<screenshots/Output of bash -n scripts aws-audit.sh.png>)

---

### Notes You Must Write (Very Important)

**1. What is stored in the checks array, and how does the loop use it?**

The checks array stores the individual audit checks that the script needs to perform, such as checking the status or configuration of AWS resources. The loop goes through each item in the array one by one, runs the corresponding check, and collects the results so they can be included in the final audit report.

**2. Why does every AWS CLI call in this script use `--query` and `--output text` instead of parsing raw JSON?**

Every AWS CLI call uses --query and --output text to return only the specific information the audit needs in a simple, consistent format. This makes the output easier for the script to process and avoids the extra complexity of parsing large raw JSON responses.

**3. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

The script uses different exit codes to clearly distinguish the audit result levels. HEALTHY indicates that the checks passed, WARN indicates a condition that needs attention but is not necessarily critical, and FAIL indicates a serious issue that requires action. Different exit codes also allow CI/CD pipelines or other automation tools to detect and respond appropriately to each outcome.

---

# Task 5 — Run the Baseline Audit

## Goal

Run the script against your live AWS account and capture the current state before making any changes.

### Evidence

#### Screenshot 8 — Output of `./scripts/aws-audit.sh` showing your Full Name and all five checks

![alt text](<screenshots/Output of scripts aws-audit.sh.png>)

---

#### Screenshot 9 — Output showing the captured exit code and final summary

![alt text](<screenshots/Output showing the captured exit code and final summaryy.png>)

---

### Notes You Must Write (Very Important)

**1. What is the overall status of your baseline audit?**

The overall status of my baseline audit was FAIL.

PASS: 3 checks
WARN: 1 check
FAIL: 1 check
Overall Status: FAIL
Exit Code: 2

**2. Did any check return FAIL or WARN? If so, which one, and what evidence did it show?**

Yes. The baseline audit returned 2 FAILs and 1 WARN:

FAIL – S3 Public Access: The bucket pravin-portfolio-favour-eu-central-1 did not fully block public ACLs. Evidence: BlockPublicAcls=False and IgnorePublicAcls=False.
FAIL – SSH Exposure: 2 security group rules allowed SSH (port 22) from 0.0.0.0/0.
WARN – EBS Encryption: 2 EBS volumes were not encrypted.

**3. If every check passed, what does that tell you about the security posture of your account so far?**

S3 public ACL access is properly blocked.
SSH (port 22) is not open to the entire internet.
MySQL (port 3306) is not open to the entire internet.
The RDS instance is not publicly accessible.
EBS volumes are encrypted.

---

# Task 6 — Build and Run the /aws-audit Skill

## Goal

Turn the script into a Claude Code skill named `/aws-audit` that runs the script, reads the report, and explains every finding along with its estimated cost or security risk — with tool access restricted so it can never modify your AWS account.

### Evidence

#### Screenshot 10 — `SKILL.md` showing the frontmatter, tool restrictions, and safety rules

![alt text](<screenshots/SKILL.md showing the frontmatter tool restrictions and safety rules.png>)

---

#### Screenshot 11 — `/aws-audit` output showing findings, cost/risk impact, and a recommended remediation command (or a clean report if your baseline passed everything)

![alt text](<screenshots/aws audit output showing findings, cost ,remediation.png>)

---

### Notes You Must Write (Very Important)

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

The skill has Bash, Read, and Grep because it is designed to inspect and analyze existing files and AWS resources, not modify them. It does not have Write permission to prevent Claude from automatically changing audit files or other project content. This supports a read-only, evidence-based audit process and keeps changes under human control.

**2. What part is performed by Bash, and what part is performed by Claude?**

Bash performs the actual AWS CLI commands and collects the raw evidence from the AWS environment. Claude interprets that evidence, evaluates the results against the audit checks, and explains the findings in the audit report. This separates evidence collection from analysis and reasoning.

**3. Why is estimating cost/risk impact something the AI adds on top of a plain PASS/FAIL script?**

A plain PASS/FAIL script only tells you whether a technical check succeeded or failed. AI can add context by estimating the cost, operational risk, or potential impact of a finding, helping humans understand how significant the issue may be and prioritize further investigation. The estimate should remain clearly identified as an AI assessment, not as measured AWS cost or guaranteed impact.

---

# Task 7 — Fix a Real Finding and Re-Verify

## Goal

Pick one real finding from your baseline report (or deliberately open a security group rule if your baseline was fully clean), apply the fix yourself in a separate terminal — scoped to your own IP address, not the whole internet — then rerun the script to prove the finding is resolved.

### Evidence

#### Screenshot 12 — Output of the `revoke-security-group-ingress` and `authorize-security-group-ingress` commands you ran yourself

![alt text](<screenshots/Output of the revoke security group ingress and authorize security group ingress.png>)

---

#### Screenshot 13 — Rerun of `./scripts/aws-audit.sh` showing the finding is now PASS

![alt text](<screenshots/Rerun of scripts aws audit sh showing the finding is now PASS.png>)

---

### Notes You Must Write (Very Important)

**1. Which exact finding did you fix, and what command did you run?**

I fixed the SSH-open-to-world finding, which identified inbound TCP port 22 access from 0.0.0.0/0. The affected Security Groups were sg-0bf9ee63c5071cecd (Book-Review-App-SG) and sg-0e9fe39355921da30 (launch-wizard-2). The remediation command used to remove the open SSH rule was:

aws ec2 revoke-security-group-ingress \
  --group-id sg-0e9fe39355921da30 \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0

I also restricted SSH access to my public IP using 98.97.76.9/32 where required.

**2. Why did you scope the new rule to your own IP address instead of leaving it open to `0.0.0.0/0`?**

I scoped the new SSH rule to my own public IP address using /32 because it allows SSH access only from my specific IP address. Leaving the rule as 0.0.0.0/0 would allow SSH connection attempts from any IPv4 address on the internet, unnecessarily increasing the attack surface. Using my IP address maintains the access I need while following the principle of least privilege and reducing exposure.

**3. Did Claude execute the remediation command, or did you? Why does that matter?**

I executed the remediation commands myself; Claude only recommended the commands and helped me identify the affected Security Groups. This matters because the audit workflow is designed to keep a human in control of changes to live AWS resources. Remediation commands can modify security configurations, so they should be reviewed and explicitly approved by a human before execution.


**4. Which phase of the Agentic Loop does the Bash script represent? Which phase does Claude's explanation represent? Which phase is you running the fix?**

The Bash audit script represents the Gather phase because it collects read-only evidence from my AWS environment.

Claude’s explanation represents the Analyze phase because it interprets the collected evidence, identifies findings, and explains their security or risk implications.

Me running the remediation command represents the Human Act phase because I reviewed and executed the approved change myself.

Afterward, rerunning the audit represents the Verify phase, where the updated AWS configuration is checked to confirm that the finding was resolved.

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

https://lnkd.in/p/eBt3T3Cv

---

#### Screenshot of Published LinkedIn Post

![alt text](<screenshots/linkdin screenshot week 6.png>)

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

- [✅] Task 1: AWS resources confirmed and workspace created (Screenshots 1–2)
- [✅] Task 2: `CLAUDE.md` created with project context and safety rules (Screenshot 3)
- [✅] Task 3: Claude produced a read-only five-check audit plan before any script existed (Screenshot 4)
- [✅] Task 4: `aws-audit.sh` built, executable, and passes `bash -n` (Screenshots 5–7)
- [✅] Task 5: Baseline audit captured and saved with Full Name visible (Screenshots 8–9)
- [✅] Task 6: `/aws-audit` skill loads and runs successfully with no Write permission (Screenshots 10–11)
- [✅] Task 7: A real finding was fixed by you and reverified as PASS (Screenshots 12–13)
- [✅] Skill never executed a remediation command
- [✅] New security group rule is scoped to your own IP, not `0.0.0.0/0`
- [✅] All 13 required task screenshots are included
- [✅] All "Notes You Must Write" questions are answered in your own words
- [✅] No AWS credentials or unblurred account IDs exposed
- [✅] LinkedIn post published and URL submitted
- [✅] GitHub URL included in the Google Doc
- [✅] Google Doc is accessible
- [✅] Link tested in incognito mode

---

# Final Submission

Submit your GitHub repository URL containing all assignment files, screenshots, reports, and output.

### GitHub Repository URL

Paste your GitHub repository URL here:

https://github.com/Favoured16/devops-micro-internship-pravinmishra.git

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