
## 📌 Week 5: Docker Basics & Advanced Challenge
Welcome to the Week 5 Docker Challenge! In this task, you will work with Docker concepts and tools taught by Shubham Bhaiya. This challenge covers the following topics:

Introduction and Purpose: Understand Docker’s role in modern development.
Virtualization vs. Containerization: Learn the differences and benefits.
Build Kya Hota Hai: Understand the Docker build process.
Docker Terminologies: Get familiar with key Docker terms.
Docker Components: Explore Docker Engine, images, containers, and more.
Project Building Using Docker: Containerize a sample project.
Multi-stage Docker Builds / Distroless Images: Optimize your images.
Docker Hub (Push/Tag/Pull): Manage and distribute your Docker images.
Docker Volumes: Persist data across container runs.
Docker Networking: Connect containers using networks.
Docker Compose: Orchestrate multi-container applications.
Docker Scout: Analyze your images for vulnerabilities and insights.

Complete all the tasks below and document your steps, commands, and observations in a file named solution.md. Finally, share your experience on LinkedIn using the provided guidelines.

---

## 🚀Task 1: Introduction and Conceptual Understanding


### 1️⃣ ** Write an Introduction:
- In your solution.md, provide a brief explanation of Docker’s purpose in modern DevOps.

```
Docker is a tool that helps developers package, share, and run applications easily and consistently across different computers.

🔹 Why is Docker Important in DevOps?
Works Everywhere – Docker puts an app and everything it needs (code, dependencies, settings) inside a container. This container runs the same way on any system (Windows, Mac, Linux, cloud servers).

Saves Time – No more "it works on my machine" issues! Developers can quickly set up environments without installing everything manually.

Fast & Lightweight – Containers are smaller and faster than traditional virtual machines, making development and deployment much quicker.

Easier Collaboration – Teams can share Docker containers, ensuring everyone works with the same setup.

Better Deployment – Docker allows for automated deployments, making it easier to update and scale applications with tools like Kubernetes.

💡 Example:
Imagine you're building a website. Instead of setting up servers manually, you can run a Docker container with everything pre-configured and deploy it instantly.

🎯 Summary
Docker makes software development and deployment faster, more reliable, and efficient—a perfect fit for DevOps! 🚀
```
- Compare Virtualization vs. Containerization and explain why containerization is the preferred approach for microservices and CI/CD pipelines.

```
|Feature Virtualization (VMs) | Containerization (Docker, Kubernetes)|
|-----------------------------|--------------------------------------|
|Architecture   | Uses a hypervisor to create multiple virtual machines, each with its own OS. |   Uses a container engine (like Docker) to run multiple lightweight containers on the same OS.|
|Resource Usage |  Heavy – Each VM has a full OS, consuming more CPU, RAM, and storage. | Lightweight – Containers share the same OS kernel, using fewer resources.|
|Startup Time    Slower – Booting a VM takes minutes.  |  Fast – Containers start in seconds.|
|Isolation |  Strong – Each VM runs a full OS, fully isolated. |   Moderate – Containers share the OS but are isolated at the application level.|
|Portability | Less portable – VMs are large and require specific hypervisor setups. |  Highly portable – Containers run the same way on any system (Linux, Windows, Mac, Cloud).|
|Scalability | Less efficient – Requires more resources for scaling. |  Highly efficient – Containers can be started, stopped, and scaled instantly.|
```

🖼 **Example of Your Forked Repo:** ![Forked Repository](image.png)

### 2️⃣ **Clone the Forked Repository**

```bash
# Replace <your-fork-url> with your forked repository link
git clone <your-fork-url>
cd 90DaysOfDevOps/2025/git/01_Git_and_Github_Basics
```

🖼 **Example of Cloning a Repo:** ![Cloning Repository](image-1.png)

---

## 🏗️ Task 2: Initialize a Git Repository & Create a File

### 1️⃣ **Initialize Git Repository**

```bash
mkdir week-4-challenge
cd week-4-challenge
git init
```

### 2️⃣ **Create and Commit a File**

```bash
echo "Hello, this is my Git challenge!" > info.txt
git add info.txt
git commit -m "Initial commit: Add info.txt"
```

🖼 **Hands-on Example:** ![git init & commit](image-2.png)

---

## 🔗 Task 3: Configure Remote & Push Changes

### 1️⃣ **Configure Remote URL with PAT**

```bash
# Replace <your-username> and <your-PAT> with actual values
git remote add origin https://<your-username>:<your-PAT>@github.com/<your-username>/90DaysOfDevOps.git
git push -u origin main
```

🖼 **Hands-on Example:** ![Git Push](image-3.png)

---

## 🔍 Task 4: View Git Commit History

### **Check Commit Logs**

```bash
git log
```

🖼 **Example Output:** ![Git Log](image-4.png)

---

## 🌿 Task 5: Branching & Merging

### 1️⃣ **Create and Switch to a New Branch**

```bash
git branch feature-update
git switch feature-update
```

### 2️⃣ **Modify and Commit Changes in New Branch**

```bash
echo "Adding more details to the file." >> info.txt
git add info.txt
git commit -m "Feature update: Enhance info.txt"
git push origin feature-update
```

🖼 **Hands-on Example:** ![Feature Branch](image-5.png)

### 3️⃣ **Merge Feature Branch to Main**

- Create a **Pull Request (PR)** on GitHub and merge changes.
- Optionally you can Delete the feature branch after merging (I didn't do that for now).

🖼 **Example of Opening a Pull Request:** ![Option to Open PR](image-6.png)
![Open PR Section](image-7.png)

🖼 **Example of Merging a PR:** ![Merge PR](image-8.png)
![Delete Branch Option](image-9.png)

---

## 🔒 Task 6: SSH Authentication

### 1️⃣ **Generate SSH Key**

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

### 2️⃣ **Add SSH Key to GitHub**

- Copy the key:

```bash
cat ~/.ssh/id_ed25519.pub
```

- Go to GitHub **Settings > SSH Keys**, add the key.

🖼 **Option to add new SSH Key:** ![Add SSH to GitHub](image-10.png)

### 3️⃣ **Update Remote URL to SSH**

```bash
git remote set-url origin git@github.com:<your-username>/90DaysOfDevOps.git
git push origin feature-update
```

🖼 **Example:** ![SSH Push](image-11.png)

---

## 🎯 Conclusion

🎉 Successfully completed **Week 4 Challenge** of **#90DaysOfDevOps**! This challenge deepened my understanding of **Git workflows, remote configurations, authentication, and branching strategies.**

🖼 **Final Screenshot:** ![Final Success](image-12.png)

