# Assignment 4 — Building Your AI Team

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build and configure a set of specialized AI subagents inside your project. You will learn how different models and tool permissions define agent behavior, and you will trigger two real agent delegations to analyze security and cost aspects of your Terraform infrastructure.

---

# Task 1 — Create the Agents Folder and Add Files

## Goal

Create the `.claude/agents/` directory and add all required agent files.

### Evidence

#### Screenshot 1 — VS Code sidebar showing `.claude/agents/` with all 3 files

Add your screenshot here.
![alt text](<Screenshot of vscode showing claude agent with 3 files.png>)
---

# Task 2 — Compare the Agent Configurations

## Goal

Analyze the configuration differences between the three agents and demonstrate understanding of model and tool selection.

### Written Answers

#### 1. Why does the cost optimizer use Haiku instead of Sonnet?

Add your answer here...
The cost optimizer uses Haiku instead of Sonnet because its task is relatively simple and repetitive—analyzing infrastructure for cost-saving opportunities. Haiku is faster and more cost-effective for this type of analysis, while Sonnet is better suited to more complex tasks that require deeper reasoning or generating sophisticated code. Using Haiku helps keep the workflow efficient and reduces AI usage costs without sacrificing the quality needed for the task.
---

#### 2. Why does the security auditor NOT have Write in its tools list?

Add your answer here...
The security auditor does not have the Write tool to ensure it can analyze and report security issues without modifying any files. This follows the principle of least privilege, reducing the risk of accidental or unauthorized changes. The auditor's role is to identify vulnerabilities and recommend fixes, while leaving implementation to a user or another agent with write permissions.
---

#### 3. Why does the tf-writer use `inherit` instead of a specific model?

Add your answer here...
The tf-writer uses inherit so it automatically uses the same AI model as the main Claude Code session. This keeps behavior consistent, avoids hardcoding a specific model, and allows the skill to benefit from future model upgrades without requiring any changes to its configuration.
---

### Evidence

#### Screenshot 2 — `security-auditor.md` frontmatter showing model and tools configuration
![alt text](<Screenshot showing security auditor with tools.png>)
Add your screenshot here.

---

#### Screenshot 3 — `cost-optimizer.md` frontmatter showing the model and tools configuration

Add your screenshot here.
![alt text](<Screenshot showing cost optimizer with tools.png>)
---

# Task 3 — Run the Security Auditor

## Goal

Trigger the security auditor agent and analyze the generated security report for your Terraform infrastructure.

### Evidence

#### Screenshot 4 — The delegation message showing Claude launched the security-auditor

Add your screenshot here.
![alt text](<Screenshot showing claude lauching security agents-1.png>)
---

#### Screenshot 5 — Security audit report output

Add your screenshot here.
![alt text](<Screenshot showing security report output .png>)
---

# Task 4 — Run the Cost Optimizer

## Goal

Trigger the cost optimizer agent and review the generated cost optimization report.

### Evidence

#### Screenshot 6 — The full cost optimization report

Add your screenshot here.
![alt text](<screenshots showingThe full cost optimization report.png>)
---

# Submission Instructions

- Ensure all agent files are committed in `.claude/agents/`
- Complete all written answers in your GitHub Repo
- Push final changes to your forked GitHub repository

---

## GitHub Repository URL

Paste your forked repository URL here:

<<<<<<< HEAD:week-02-agentic-ai/solution-assignment-04-subagents.md
`https://github.com/Favoured16/Ultimate-Agentic-DevOps-with-Claude-Code.git`
=======
`Add your URL here`
>>>>>>> upstream/main:week-02-agentic-ai/assignment-04-subagents.md

---

# Completion Checklist

- [ ] `.claude/agents/` folder contains all 3 agent files
- [ ] Screenshot 2 shows correct `security-auditor.md` configuration
- [ ] Screenshot 3 shows correct `cost-optimizer.md` configuration
- [ ] All 3 written answers completed 
- [ ] Security auditor executed successfully
- [ ] Cost optimizer executed successfully
- [ ] Security report is visible with findings
- [ ] Cost report is visible with recommendations
- [ ] All required screenshots added
- [ ] GitHub repo updated with agents

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*