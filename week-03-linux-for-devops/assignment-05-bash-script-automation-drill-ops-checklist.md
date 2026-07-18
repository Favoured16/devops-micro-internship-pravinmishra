# Assignment 5 — Bash Script Automation Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will practice Bash scripting by building a series of small automation scripts covering environment setup, variables, arrays, loops, file conditionals, if-else logic, and functions. These scripts form the foundation of real-world Linux automation used in DevOps, cloud, and production support environments.

---

# Task 1 — Bash Environment & Workspace Setup

## Goal

Verify that Bash is available on your system and create a clean workspace for this assignment.

### Evidence

#### Screenshot 1 — Output of `echo $SHELL` and `bash --version`

Add your screenshot here.
![alt text](<Output of echo $SHELL and bash -version.png>)
---

#### Screenshot 2 — Output of `pwd` and `ls -lah` showing the scripts directory

Add your screenshot here.
![alt text](<Output of pwd and ls  lah showing the scripts directory.png>)
---

### Notes

Answer the following in your own words:

**1. What is Bash?**

Add your answer here.
Bash (Bourne Again Shell) is a command-line shell and scripting language commonly used on Linux and Unix-based operating systems. It allows users to interact with the operating system by typing commands, automate repetitive tasks through scripts, and manage files, processes, and system resources.
---

**2. What is the difference between shell and Bash?**

Add your answer here.
Shell
A shell is a command-line interface that allows users to interact with the operating system.
It is a general term for programs that interpret and execute user commands.
It is one of the most widely used shells on Linux and Unix systems.
There are several types of shells, such as Bash, Zsh, Fish, and Korn Shell (Ksh).

While

Bash
Bash (Bourne Again Shell) is a specific type of shell.
It is one of the most widely used shells on Linux and Unix systems.
Bash provides features such as command history, tab completion, variables, loops, functions, and scripting capabilities.
---

**3. Why is it important to confirm the Bash version before writing scripts?**

Add your answer here.
It is important to confirm the Bash version before writing scripts because different Bash versions support different features and syntax. A script that works on a newer version of Bash may fail on an older version if it uses unsupported commands or functionality. Checking the Bash version helps ensure compatibility, prevents unexpected errors, and allows you to write scripts that will run reliably on the target system.
---

# Task 2 — Your First Bash Script

## Goal

Create your first Bash script, make it executable, and run it from the terminal.

### Evidence

#### Screenshot 1 — Content of `first-script.sh`

Add your screenshot here.
![alt text](<My First Bash script.png>)
---

#### Screenshot 2 — Output of `./first-script.sh`

Add your screenshot here.
![alt text](<Output of First-script.sh.png>)

---

#### Screenshot 3 — Output of `ls -l first-script.sh` showing executable permission

Add your screenshot here.
![alt text](<Output of `ls -l first-script.sh` showing executable permission.png>)
---

### Notes

Answer the following in your own words:

**1. What is the purpose of `#!/bin/bash`?**

Add your answer here.
#!/bin/bash is called a shebang (or hashbang). It tells the operating system which interpreter should execute the script.
---

**2. Why do we use `chmod +x` before running a script?**

Add your answer here.
We use chmod +x before running a script to make the script executable.

By default, a script file may only have read and write permissions. The +x option adds the execute permission, allowing the operating system to recognize and run the file as a program.
---

**3. What is the difference between running a script using `./script.sh` and `bash script.sh`?**

Add your answer here.
./script.sh
Executes the script as a program.
The script must have execute permission (chmod +x script.sh).
The script typically relies on the shebang (e.g., #!/bin/bash) to determine which interpreter to use.

While
bash script.sh
Runs the script using the Bash interpreter directly.
The script does not need execute permission.
Ignores the shebang and explicitly uses Bash to run the script.
---

# Task 3 — Variables: User Information Script

## Goal

Use variables to store and display user-related information.

### Evidence

#### Screenshot 1 — Content of `user-info.sh`

Add your screenshot here.
![alt text](<Content of user-info.sh.png>)
---

#### Screenshot 2 — Output of `./user-info.sh`

Add your screenshot here.
![alt text](<Output of user-info.sh.png>)
---

### Notes

Answer the following in your own words:

**1. What is a variable in Bash?**

Add your answer here.
A variable in Bash is a named storage location used to hold data, such as text, numbers, or the output of commands. Variables make scripts more flexible and reusable by allowing values to be stored and referenced throughout the script.
---

**2. Why should we avoid spaces around the `=` sign when creating variables?**

Add your answer here.
In Bash, you should avoid spaces around the = sign when creating variables because Bash treats spaces as argument separators, not as part of a variable assignment.
---

**3. How do you access the value stored inside a Bash variable?**

Add your answer here.
You access the value stored in a Bash variable by prefixing the variable name with a dollar sign ($).
---

# Task 4 — Arrays & Loops: Tools Checklist Script

## Goal

Use arrays and loops to print a checklist of tools used in Bash scripting.

### Evidence

#### Screenshot 1 — Content of `tools-checklist.sh`

Add your screenshot here.
![alt text](<Content Of tools checklist sh.png>)
---

#### Screenshot 2 — Output of `./tools-checklist.sh`

Add your screenshot here.
![alt text](<Output of tools checklist sh.png>)
---

### Notes

Answer the following in your own words:

**1. What is an array in Bash?**

Add your answer here.
An array in Bash is a variable that can store multiple values under a single name. Instead of creating separate variables for each value, you can group related values together in an array and access them using their index.
---

**2. Why are arrays useful in scripts?**

Add your answer here.
Arrays are useful because they enable you to store multiple values in one variable, simplify data management, reduce repetitive code, and make it easier to process collections of items using loops.
---

**3. What does `"${tools[@]}"` mean?**

Add your answer here.
"${tools[@]}" is a Bash expression used to access all the elements of an array, with each element treated as a separate item. It is commonly used when looping through an array or passing its elements as arguments to a command.
---

**4. What is the purpose of the `for` loop in this script?**

Add your answer here.
In a Bash script, the 'for' loop is used to repeat a set of commands for each item in a list or array. Its purpose is to automate repetitive tasks and process multiple values one by one.
---

# Task 5 — Loops: Number Counter Script

## Goal

Use loops to repeat a task multiple times.

### Evidence

#### Screenshot 1 — Content of `counter.sh`

Add your screenshot here.
![alt text](<Content Of counter sh.png>)
---

#### Screenshot 2 — Output of `./counter.sh`

Add your screenshot here.
![alt text](<Output of counter sh.png>)
---

### Notes

Answer the following in your own words:

**1. What is a loop?**

Add your answer here.
A loop is a programming construct that repeats a set of commands or instructions until all items have been processed or a specified condition is met. Loops help automate repetitive tasks, making scripts more efficient and reducing the need to write the same code multiple times.
---

**2. Why do we use loops in Bash scripting?**

Add your answer here.
We use loops in Bash scripting to automate repetitive tasks by executing the same set of commands multiple times. Instead of writing the same code repeatedly, a loop processes each item in a list, array, or range automatically, making scripts more efficient, readable, and easier to maintain.
---

**3. How many times did the loop run in your script?**

Add your answer here.
The loop ran five times because i gave it five values:
1 2 3 4 5

---

**4. What would you change if you wanted the loop to run 10 times?**

Add your answer here.
I would add the numbers 6 to 10 to the for loop:
for number in 1 2 3 4 5 6 7 8 9 10

---

# Task 6 — Files & Conditionals: File Validation Script

## Goal

Use file checks and conditionals to verify whether files and directories exist.

### Evidence

#### Screenshot 1 — Output of `ls -lah ../test-folder`

Add your screenshot here.
!![alt text](<Output of ls lah test folder-1.png>)
---

#### Screenshot 2 — Content of `file-check.sh`

Add your screenshot here.
![alt text](<Content Of file check sh.png>)
---

#### Screenshot 3 — Output of `./file-check.sh`

Add your screenshot here.
![alt text](<Output Of file check sh.png>)
---

### Notes

Answer the following in your own words:

**1. What does `-d` check in Bash?**

Add your answer here.
In Bash, the -d option is used to check whether a specified path is a directory.
It is commonly used in an if statement to verify that a directory exists before performing operations on it.
---

**2. What does `-f` check in Bash?**

Add your answer here.
In Bash, the -f option is used to check whether a specified path exists and is a regular file.

It is commonly used in an if statement to verify that a file exists before reading, copying, or modifying it.
---

**3. Why should file and directory paths be stored in variables?**

Add your answer here.
File and directory paths should be stored in variables to make Bash scripts more readable, maintainable, and reusable. Instead of hardcoding the same path multiple times, you define it once in a variable and reference it throughout the script. This makes it easier to update the path if it changes and reduces the risk of errors.
---

**4. What happens if the file does not exist?**

Add your answer here.
If the file does not exist, the -f test returns false, and the commands in the else block (if provided) are executed. This allows the script to handle the situation gracefully instead of failing with an error.
---

# Task 7 — Conditionals: Pass or Retry Script

## Goal

Use if-else conditionals to make decisions based on a variable value.

### Evidence

#### Screenshot 1 — Content of `score-check.sh` with `score=85`

Add your screenshot here.
![alt text](<Content Of core check.sh With score=85.png>)
---

#### Screenshot 2 — Output showing `Result: Pass`

Add your screenshot here.
![alt text](<Output showing Result Pass.png>)
---

#### Screenshot 3 — Content of `score-check.sh` with `score=55`

Add your screenshot here.
![alt text](<Content Of core check.sh With score=85-1.png>)
---

#### Screenshot 4 — Output showing `Result: Retry`

Add your screenshot here.
![alt text](<Output showing Result Retry.png>)
---

### Notes

Answer the following in your own words:

**1. What is the purpose of if-else in Bash?**

Add your answer here.
The if-else statement in Bash is used to make decisions in a script by executing different blocks of code based on whether a specified condition is true or false.
---

**2. What does `-ge` mean?**

Add your answer here.
In Bash, -ge is a comparison operator that means "greater than or equal to." It is used to compare two integer values in conditional statements.
---

**3. Why should conditions be tested with different values?**

Add your answer here.
Conditions should be tested with different values to ensure that a Bash script behaves correctly in all possible scenarios. Testing both true and false conditions helps verify that the script executes the correct logic, handles edge cases, and reduces the risk of unexpected errors.
---

**4. How can conditionals help in automation scripts?**

Add your answer here.
Conditionals help in automation scripts by enabling the script to make decisions based on specific conditions. They allow the script to perform different actions depending on the state of the system, user input, or the outcome of a command, making automation more intelligent and reliable.
---

# Task 8 — Functions: Final Bash Automation Script

## Goal

Create a final Bash script using functions to organize reusable code.

### Evidence

#### Screenshot 1 — Content of `final-automation.sh`

Add your screenshot here.
![alt text](<Content Of final automation sh.png>)
---

#### Screenshot 2 — Output of `./final-automation.sh`

Add your screenshot here.
![alt text](<Output Of final automation sh.png>)
---

#### Screenshot 3 — Output of `ls -lah` showing all created scripts

Add your screenshot here.
![alt text](<Output of ls lah showing all created scripts.png>)
---

### Notes

Answer the following in your own words:

**1. What is a function in Bash?**

Add your answer here.
A function in Bash is a named block of code that performs a specific task. Instead of writing the same commands multiple times, you can define them once in a function and call the function whenever needed. This makes scripts more organized, reusable, and easier to maintain.
---

**2. Why are functions useful in scripts?**

Add your answer here.
Functions are useful in Bash scripts because they allow you to group reusable commands into a single block of code. This reduces repetition, makes scripts easier to read and maintain, and simplifies updates since changes only need to be made in one place.
---

**3. Which functions did you create in this script?**

Add your answer here.
The functions I created in this script were print_header,print_user_details, check_files, and print_tools. 
Each function was designed to perform a specific task: print_header displays the script title, print_user_details prints the user's information, check_files verifies the existence of the required directory and file using conditionals, and print_tools displays the list of tools stored in an array using a loop. Organizing these tasks into separate functions made the script more structured, reusable, and easier to maintain.
---

**4. How does this final script combine variables, arrays, loops, conditionals, files, and functions?**

Add your answer here.
This final script combines several core Bash scripting concepts to automate common tasks efficiently. It uses variables to store reusable values such as the user's name, assignment name, and file paths. An array stores a list of tools, while a for loop iterates through the array to display each tool. Conditional statements (if-else) check whether the required directory and file exist before displaying the appropriate message. The script works with files and directories by validating their existence using the -d and -f tests. Finally, functions organize related tasks into reusable blocks of code, making the script more structured, readable, and easier to maintain. Together, these components demonstrate how Bash can automate tasks in a clean and efficient manner.
---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/favour-chibundu-323793353_aws-ec2-reactjs-activity-7483854754897518592-5PXQ?utm_source=share&utm_medium=member_desktop&rcm=ACoAAFg28wsB9vXuv3Kyn9OulOUEyNs4CtNMXQs`

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.
!![alt text](<Published LinkedIn post.png>)
---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- All script files must be created and run successfully
- Required notes must be answered clearly for every task
- Do not expose sensitive information (keys, passwords, credentials)

---

# Completion Checklist

- [ ] Task 1: Environment setup verified, workspace created (Screenshots 1–2, Notes answered)
- [ ] Task 2: First script created, executed, permissions verified (Screenshots 1–3, Notes answered)
- [ ] Task 3: Variables script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 4: Arrays and loops script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 5: Counter loop script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 6: File validation script created and run (Screenshots 1–3, Notes answered)
- [ ] Task 7: Pass/Retry conditional script tested with both values (Screenshots 1–4, Notes answered)
- [ ] Task 8: Final automation script created and run (Screenshots 1–3, Notes answered)
- [ ] All scripts run without errors
- [ ] Full Name visible in all required screenshots
- [ ] LinkedIn post published and URL submitted
- [ ] No sensitive data exposed

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