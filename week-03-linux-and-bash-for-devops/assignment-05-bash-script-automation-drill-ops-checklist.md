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

[Assignment screenshot](screenshots/image15a.png)

---

#### Screenshot 2 — Output of `pwd` and `ls -lah` showing the scripts directory

[Assignment screenshots](screenshots/image15b.png)

---

### Notes

Answer the following in your own words:

**1. What is Bash?**

Bash is a program that allows us to run commands and write scripts to automate tasks.

---

**2. What is the difference between shell and Bash?**

A shell is a program that interprets commands and provides an interface for interacting with the OS. Bash is one type of shell.

---

**3. Why is it important to confirm the Bash version before writing scripts?**

It helps us know which Bash features are available and avoid compatibility problems.

---

# Task 2 — Your First Bash Script

## Goal

Create your first Bash script, make it executable, and run it from the terminal.

### Evidence

#### Screenshot 1 — Content of `first-script.sh`

[Assignment screenshot](screenshots/image16a.png)

---

#### Screenshot 2 — Output of `./first-script.sh`

[Assignment screenshot](screenshots/image16b.png)

---

#### Screenshot 3 — Output of `ls -l first-script.sh` showing executable permission

[Assignment screenshot](screenshots/image16b.png)

---

### Notes

Answer the following in your own words:

**1. What is the purpose of `#!/bin/bash`?**

It is the shebang line. It tells the OS to use Bash to run the script.

---

**2. Why do we use `chmod +x` before running a script?**

It gives the script permission to run as an executable file.
---

**3. What is the difference between running a script using `./script.sh` and `bash script.sh`?**

'./script.sh' runs the script directly and needs executable permission. 
'bash script.sh' runs it using Bash without needing that permission.

---

# Task 3 — Variables: User Information Script

## Goal

Use variables to store and display user-related information.

### Evidence

#### Screenshot 1 — Content of `user-info.sh`

[Assignment Screenshot](screenshots/image17a.png)

---

#### Screenshot 2 — Output of `./user-info.sh`

[Assignment screenshot](screenshots/image17b.png)

---

### Notes

Answer the following in your own words:

**1. What is a variable in Bash?**

A variable is a name that holds a value, used to store information, such as a username, age, server IP and so on that will be used later in the script.

---

**2. Why should we avoid spaces around the `=` sign when creating variables?**

Because Bash does not allow spaces around '=' when assigning a value. If there is a space, it tries to run it as command and argument leading to failure. So it is best to avoid spaces around it.
For example, name="Peter" is correct.

---

**3. How do you access the value stored inside a Bash variable?**

You access the value by using the variable name with a $ sign, such as '$name'.

---

# Task 4 — Arrays & Loops: Tools Checklist Script

## Goal

Use arrays and loops to print a checklist of tools used in Bash scripting.

### Evidence

#### Screenshot 1 — Content of `tools-checklist.sh`

[Assignment screenshot](screenshots/image18a.png)
---

#### Screenshot 2 — Output of `./tools-checklist.sh`

[Assignment screenshot](screenshots/image18b.png)

---

### Notes

Answer the following in your own words:

**1. What is an array in Bash?**

An array is a variable that can store multiple values all under one name.

For this script, the tools array stores several Linux and Bash tools.
Example:
tools=("bash" "nano" "chmod" "echo" "ls" "pwd")

---

**2. Why are arrays useful in scripts?**

They allow us to store and work with many related items, such as a list of tools.

---

**3. What does `"${tools[@]}"` mean?**

It means all the values stored in the tools array.
For this script, it gives the loop access to every tool in the array.
The double quotes help keep each array item as a separate value. This is especially important when an item contains spaces.


---

**4. What is the purpose of the `for` loop in this script?**

It starts the loop, goes through each tool in the array and prints it.

---

# Task 5 — Loops: Number Counter Script

## Goal

Use loops to repeat a task multiple times.

### Evidence

#### Screenshot 1 — Content of `counter.sh`

[Assignment screenshot](screenshots/image19a.png)

---

#### Screenshot 2 — Output of `./counter.sh`

[Assignment screenshot](screenshots/image19b.png)

---

### Notes

Answer the following in your own words:

**1. What is a loop?**

A loop is a way/what can be used to repeat a task several times.

---

**2. Why do we use loops in Bash scripting?**

They save time by repeating tasks automatically instead of writing the same command many times.

---

**3. How many times did the loop run in your script?**

The loop ran 5 times.

---

**4. What would you change if you wanted the loop to run 10 times?**

I would add the numbers 6 to 10 to the for loop:
for number in 1 2 3 4 5 6 7 8 9 10
do
   	echo "Step $number completed"
done

---

# Task 6 — Files & Conditionals: File Validation Script

## Goal

Use file checks and conditionals to verify whether files and directories exist.

### Evidence

#### Screenshot 1 — Output of `ls -lah ../test-folder`

[Assignment screenshot](screenshots/image20b.png)

---

#### Screenshot 2 — Content of `file-check.sh`

[Assignment screenshot](screenshots/image20a.png)

---

#### Screenshot 3 — Output of `./file-check.sh`

[Assignment screenshot](screenshots/image20b.png)

---

### Notes

Answer the following in your own words:

**1. What does `-d` check in Bash?**

It checks whether a given path/directory exists.

---

**2. What does `-f` check in Bash?**

It checks whether a file exists.

---

**3. Why should file and directory paths be stored in variables?**

It makes the script easier to read and update.
If a path changes, we only need to update the variable instead of changing the same path in several places.
---

**4. What happens if the file does not exist?**

The f-check becomes false, so the script can display a message saying the file does not exist, the following message will be displayed:
File does not exist: ../test-folder/student-info.txt

---

# Task 7 — Conditionals: Pass or Retry Script

## Goal

Use if-else conditionals to make decisions based on a variable value.

### Evidence

#### Screenshot 1 — Content of `score-check.sh` with `score=85`

[Assignment screenshot](screenshots/image21a.png)

---

#### Screenshot 2 — Output showing `Result: Pass`

[Assignment screenshot](screenshots/image21b.png)

---

#### Screenshot 3 — Content of `score-check.sh` with `score=55`

[Assignment screenshot](screenshots/image22a.png)

---

#### Screenshot 4 — Output showing `Result: Retry`

[Assignment screenshot](screenshots/image22b.png)

---

### Notes

Answer the following in your own words:

**1. What is the purpose of if-else in Bash?**

It allows the script to make decisions based on a condition.If the given condition is true, it runs one set of commands. If the condition is false, it runs another set of commands.

---

**2. What does `-ge` mean?**

It means greater than or equal to.

---

**3. Why should conditions be tested with different values?**

To make sure the script works correctly in different situations.

---

**4. How can conditionals help in automation scripts?**

They allow scripts to make decisions automatically based on the current situation, such as checking whether a task passed or failed.

---

# Task 8 — Functions: Final Bash Automation Script

## Goal

Create a final Bash script using functions to organize reusable code.

### Evidence

#### Screenshot 1 — Content of `final-automation.sh`

[Assignment screenshot](screenshots/image23a.png)

---

#### Screenshot 2 — Output of `./final-automation.sh`

[Assignment screenshot](screenshots/image23b.png)

---

#### Screenshot 3 — Output of `ls -lah` showing all created scripts

[Assignment screenshot](screenshots/image23b.png)

---

### Notes

Answer the following in your own words:

**1. What is a function in Bash?**

A function is a group of commands used to perform specific task that can be run by calling the function name and can be reused in a script.

---

**2. Why are functions useful in scripts?**

They make scripts easier to organize, read, and maintain.

---

**3. Which functions did you create in this script?**

I created four functions for the main tasks in my script:
print_header prints the assignment header.
print_user_details prints my full name and the assignment name.
check_files checks whether the required directory and file exist.
print_tools uses a loop to print each tool stored in the array.

---

**4. How does this final script combine variables, arrays, loops, conditionals, files, and functions?**

It uses variables to store information, arrays to store tools, loops to repeat tasks and print them one by one, conditionals to make decisions, file checks to verify files, and finally, it uses functions to organize the codes and call them in the correct order to complete the automation script.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/angela-chibuike_devops-cloudcomputing-linux-activity-7420345062426992641-U8MD?utm_source=share&utm_medium=member_desktop&rcm=ACoAADn-PSABhIre4cnftTYXk433XaYMG-l_k9Y `

---

#### Screenshot — Published LinkedIn post

[Assignment screenshot](screenshots/image24a.png)

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

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*