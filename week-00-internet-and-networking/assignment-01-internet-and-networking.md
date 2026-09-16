# Week 00 - Internet and Networking

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

# 🧑‍💻 Task 1: Using ChatGPT as Your Learning Assistant

## Scenario

You're new to DevOps and will frequently encounter technical questions. ChatGPT can be your learning companion.

## Your Task

Write a clear ChatGPT prompt to help you understand:

> "What is a protocol in networking? Explain with a simple real-life example."

Take a screenshot of your interaction showing:

* Your detailed prompt (with clear expectations)
* ChatGPT's simplified response with an example

## Screenshot

Save your screenshot in the `screenshots` folder and update the file name below.

[Task 1 Screenshot](screenshots/screenshot-1-chatgpt.png)
[Task 1 Screenshot](screenshots/screenshot-2-chatgpt.png) 

Replace `task-1-chatgpt.png` with your actual screenshot file name.

---

## What I Learned (2–3 lines)

I learned that a networking protocol is a set of rules that devices follow to communicate and exchange information over a network. I also learned that protocols work like rules in a conversation, ensuring that devices understand and respond to each other correctly.

---

# 🌐 Task 2: Internet and Networking

## Scenario

Your friend is launching an online bookstore named **EpicReads**.

He asked you to explain how users globally can access his website hosted in Finland.

## Your Task

Write a short explanation (**100–150 words**) that includes:

* Packet Switching
* IP Address
* TCP/IP
* HTTP/HTTPS

💡 **Tip:** You may use ChatGPT (as demonstrated in Task 1) to refine your explanation.

## Answer

When someone in the USA or any part of the world visits EpicReads, hosted in Finland, their computer first finds the site’s IP address, which works like a unique street address on the internet. The request to view the page is broken into small chunks called packets and sent across the network. This process, which is called packet switching, happens in both directions — for the request going to Finland and for the website data coming back. The TCP/IP protocols make this work smoothly: IP tells each packet where to go, and TCP ensures all packets arrive complete and in the correct order. Once the packets reach the Finnish server, it sends the web page back using HTTP or the secure HTTPS, which defines how the page’s text, images, and other elements are delivered so the browser can display them. All of this happens in a fraction of a second.


---

# 🏗️ Task 3: Application Architecture & Stack

## Scenario

EpicReads bookstore has two application versions:

### Two-Tier Application

* Frontend
* Database

### Three-Tier Application

* Frontend
* Backend
* Database

## Your Task

* Draw simple diagrams (hand-drawn or tool-based such as draw.io)
* Label each layer clearly
* List at least two common technologies or tools used for each layer
* Submit a screenshot or photo clearly showing your own drawing

## Diagram Screenshot / Photo

Save your diagram image in the `screenshots` folder and update the file name below.

[Application Architecture Diagram](screenshots/task3-drawing.png)


Replace `task-3-diagram.png` with your actual diagram file name.
---

## Technologies Used

### Frontend

* Next.js
* Tailwind.css

### Backend

* Nodejs
* Django

### Database

* MySql
* MongoDB


---

# 🌍 Task 4: Domain Name & DNS (Basic Concepts)

## Scenario

Your friend's bookstore **EpicReads** is currently accessible through:

```text
52.172.142.222:3000
```

He purchased the domain:

```text
epicreads.com
```

## Your Task

In **50–100 words**, explain in your own words:

1. What is DNS (Domain Name System)?
2. Which DNS record type should be used to connect the domain to the given IP, and why?

## Answer

1. *DNS (Domain Name System)* is like the internet’s phonebook or address storage system which keeps a record of the human readable address for various websites. It makes it easy to remember and access a website using domain names eg. google.com without struggling to remember the IP address numbers, and translates this domain name to the IP address for our browser to find and serve up the right server.

2. My friend should use an ‘A’ Record type to connect his domain, this is because an A record maps a domain directly to an IPv4 address, ensuring that when someone types epicreads.com, the browser knows to reach the server at the IP address (52.172.142.222), allowing users to access the bookstore.


---

# 💻 Task 5: Visual Studio Code Setup (Hands-on)

## Your Task

Install Visual Studio Code (if not already installed).

Take a screenshot of your VS Code environment showing:

* Terminal open inside VS Code
* Running a basic command:

### Windows

```powershell
dir
```

### Linux / macOS

```bash
pwd
ls
```

* Your selected VS Code theme clearly visible

⚠️ **Important:** The screenshot must show your username or another identifiable detail to confirm it is your environment.

## Screenshot

Save your screenshot in the `screenshots` folder and update the file name below.

[VS Code Setup Screenshot](screenshots/task5-vscode.png)



---

# 🔗 Task 6: Publish Your Assignment as a LinkedIn Post

## Objective

Publishing on LinkedIn helps you:

* Build your professional online presence
* Reinforce your learning
* Document your DevOps journey publicly

## Your Task

Summarize your answers from Tasks 1–5 into a LinkedIn post.

Clearly structure your post into the following sections:

* ChatGPT
* Internet & Networking
* App Architecture
* DNS
* VS Code Setup

Add the following credit note at the end of your post:

> **P.S. This post is part of the DevOps Micro Internship (DMI) with Agentic AI — Cohort 3 — by Pravin Mishra. My graded progress is public: https://dmi.pravinmishra.com/s/YOUR-GITHUB-USERNAME.html · Start your DevOps journey: https://dmi.pravinmishra.com/?utm_source=student&utm_medium=ps-linkedin&utm_campaign=cohort3**

---

## LinkedIn Post URL

Paste your LinkedIn post URL here:

```text
https://www.linkedin.com/posts/angela-chibuike_i-got-into-a-new-devops-program-recently-activity-7361697400802971648-Dc_i?utm_source=share&utm_medium=member_android&rcm=ACoAADn-PSABhIre4cnftTYXk433XaYMG-l_k9Y 
```

---

## LinkedIn Post Backup Copy

Paste the full text of your LinkedIn post here:

I Got into a new Devops program recently, which i'm grateful for as it'll help me in my quest to get more hands-on training via building projects. 

The first week was all about giving us a refresher and introductory course on Some basic concepts like;
  • Internet and Networking
  • App Architecture Stack
  • DNS basics 
  • VsCode  and 
  • Prompting with AI, with assignments to get us up to speed.

Applying this knowledge, I completed Week 0 tasks, some of which involved explaining how users access the "EpicReads" website hosted in Finland from the USA; from requests travelling as data packets via packet switching, guided by IP addresses and reassembled reliably through TCP/IP, with HTTP/HTTPS delivering the final webpage.
Next, I compared application architectures - sketching a two-tier system (Frontend → Database) and three-tier setup (Frontend → Backend → Database), noting tools like Next.js for frontend, Node.js/Django for backends, and MongoDB for databases, among other tasks.

It has been an exciting experience so far, looking forward to learning more.

𝗣.𝗦. This post is part of the DevOps Micro Internship (DMI) — Self-Paced Engineer Track — by Pravin Mishra. My graded progress is public: https://lnkd.in/dpxu7PJU · 
Start your DevOps journey: https://lnkd.in/dPHQUBcC

---

# Reflection – Week 0

### What did you find easy?

Understanding basic networking concepts such as protocols, IP addresses, DNS, and how websites communicate over the internet was relatively easy, especially when using real-life examples.

---

### What was difficult?

Understanding how the different networking and application architecture concepts connect and work together.

---

### What will you improve next week?

I will improve my understanding by practicing more, asking questions when I get stuck, and spending more time working hands-on with the tools and concepts I learn.

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.


## 📌 Resources

- 🌐 **DMI Official Website:** https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 **University:** https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 **Discord Community:** https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 **Blog:** https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ **YouTube Playlist (DMI Cohort 3):** https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 **Pravin Mishra (LinkedIn):** https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 **CloudAdvisory (LinkedIn):** https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track*