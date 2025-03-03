
## 📌 Week 5: Docker Basics & Advanced Challenge
Welcome to the Week 5 Docker Challenge! In this task, you will work with Docker concepts and tools taught by Shubham Bhaiya. This challenge covers the following topics:

- **Introduction and Purpose**: Understand Docker’s role in modern development.
- **Virtualization vs. Containerization**: Learn the differences and benefits.
- **Build Kya Hota Hai**: Understand the Docker build process.
- **Docker Terminologies**: Get familiar with key Docker terms.
- **Docker Components**: Explore Docker Engine, images, containers, and more.
- Project Building Using Docker: Containerize a sample project.
- Multi-stage Docker Builds / Distroless Images: Optimize your images.
- Docker Hub (Push/Tag/Pull): Manage and distribute your Docker images.
- Docker Volumes: Persist data across container runs.
- Docker Networking: Connect containers using networks.
- Docker Compose: Orchestrate multi-container applications.
- Docker Scout: Analyze your images for vulnerabilities and insights.

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

|Feature Virtualization (VMs) | Containerization (Docker, Kubernetes)|
|-----------------------------|--------------------------------------|
|Architecture   | Uses a hypervisor to create multiple virtual machines, each with its own OS. |   Uses a container engine (like Docker) to run multiple lightweight containers on the same OS.|
|Resource Usage |  Heavy – Each VM has a full OS, consuming more CPU, RAM, and storage. | Lightweight – Containers share the same OS kernel, using fewer resources.|
|Startup Time    Slower – Booting a VM takes minutes.  |  Fast – Containers start in seconds.|
|Isolation |  Strong – Each VM runs a full OS, fully isolated. |   Moderate – Containers share the OS but are isolated at the application level.|
|Portability | Less portable – VMs are large and require specific hypervisor setups. |  Highly portable – Containers run the same way on any system (Linux, Windows, Mac, Cloud).|
|Scalability | Less efficient – Requires more resources for scaling. |  Highly efficient – Containers can be started, stopped, and scaled instantly.|


### Task 2️⃣ ****Create a Dockerfile for a Sample Project

1. Select or Create a Sample Application:

- Choose a simple application (for example, a basic Node.js, Python, or Java app that prints “Hello, Docker!” or serves a simple web page).

```
#### Base image (OS)
FROM python:3.9-slim
#### Working directory
WORKDIR /app
#### Copy src code to container
COPY . .
#### Run the build commands
RUN pip install -r requirements.txt
#### expose port 80
EXPOSE 80
#### serve the app / run the app (keep it running)
CMD ["python","run.py"]
```
2. Write a Dockerfile:
- Create a Dockerfile that defines how to build an image for your application.
- Include comments in your Dockerfile explaining each instruction.
- Build your image using:
```docker build -t <your-username>/sample-app:latest .
```
- Verify Your Build:
- Run your container locally to ensure it works as expected:
```docker run -d -p 8080:80 <your-username>/sample-app:latest
```
- Verify the container is running with:
```docker ps
```
- Check logs using:
```docker logs <container_id>
```
![pic](https://github.com/user-attachments/assets/94660dc8-5589-40a8-a10b-fc1fedb47227)
![pic2](https://github.com/user-attachments/assets/ec801d2b-558c-4a69-9647-11846dd8d9c3)


## 🏗️ Task 3: Explore Docker Terminologies and Components
1. Document Key Terminologies:
- In your solution.md, list and briefly describe key Docker terms such as image, container, Dockerfile, volume, and network.
|Docker Term | Description |
|------------|-------------|
|Image | A pre-packaged application with all dependencies.|
|Container | A running instance of an image, providing an isolated environment.|
|Dockerfile | A script with instructions to build a Docker image.|
|Volume | Persistent storage for containers.|
|Network | Enables secure communication between containers.|

- Explain the main Docker components (Docker Engine, Docker Hub, etc.) and how they interact.
|Component | Description|
|----------|------------|
|Docker Engine | Core service that builds, runs, and manages containers.|
|Docker Image  |  A pre-packaged application that serves as a template for containers.|
|Docker Container  |  A running instance of a Docker image.|
|Dockerfile | A script with instructions to build a custom image.|
|Docker Hub | A cloud registry for storing and sharing images.|
|Docker Compose | A tool to manage multi-container applications.|
|Docker Network | Enables secure communication between containers.|
|Docker Volume | Provides persistent storage for containers.|

## Task 4 : Optimize Your Docker Image with Multi-Stage Builds

1. Implement a Multi-Stage Docker Build:
- Modify your existing Dockerfile to include multi-stage builds.
- Aim to produce a lightweight, distroless (or minimal) final image.
```
#### Build Stage
FROM python:3.9-slim AS builder
WORKDIR /app
#### Copy dependency file first for efficient caching
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt --target=/app/deps
#### Copy application source code
COPY . .
#### Final Distroless Stage
FROM gcr.io/distroless/python3-debian11
WORKDIR /app
#### Copy dependencies from the builder stage
COPY --from=builder /app/deps /app/deps
COPY --from=builder /app .
#### Set environment variables
ENV PYTHONUNBUFFERED=1
ENV PYTHONPATH="/app/deps"
#### Expose the application port
EXPOSE 80
#### Run the application
CMD ["run.py"]
```
2. Compare Image Sizes:
- Build your image before and after the multi-stage build modification and compare their sizes using:
  docker images`
![pic3 (2)](https://github.com/user-attachments/assets/46c9dbfe-4c11-473a-a548-ae40f439dffa)
![pic4](https://github.com/user-attachments/assets/9ad158d5-f2b9-4024-b1f6-c6af80b80352)

3. Document the Differences:
- Explain in solution.md the benefits of multi-stage builds and the impact on image size.
```A multi-stage build is a Docker feature that allows you to use multiple FROM statements in a Dockerfile, where each stage produces an intermediate container, and only the final stage is included in the final image.
🔹 Multi-stage builds significantly reduce image size → Faster deployment & lower storage costs.
🔹 Improved security by removing unnecessary dependencies.
🔹 Faster CI/CD pipelines due to smaller image pull/push times.
🔹 More maintainable Dockerfiles with clear separation of build and runtime stages.
```

## 🔗 Task 5: Manage Your Image with Docker Hub
1. Tag Your Image:
- Tag your image appropriately:
```
docker tag <your-username>/sample-app:latest <your-username>/sample-app:v1.0
```
2. Push Your Image to Docker Hub:
- Log in to Docker Hub if necessary:
```
docker login
```
- push the image:
```
docker push <your-username>/sample-app:v1.0
```
3. (Optional) Pull the Image:
- Verify by pulling your image:
```
docker pull <your-username>/sample-app:v1.0
```


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

