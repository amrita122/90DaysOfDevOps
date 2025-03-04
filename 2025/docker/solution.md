
## Week 5: Docker Basics & Advanced Challenge
Welcome to the Week 5 Docker Challenge! In this task, you will work with Docker concepts and tools taught by Shubham Bhaiya. This challenge covers the following topics:

- **Introduction and Purpose**: Understand Dockerâ€™s role in modern development.
- **Virtualization vs. Containerization**: Learn the differences and benefits.
- **Build Kya Hota Hai**: Understand the Docker build process.
- **Docker Terminologies**: Get familiar with key Docker terms.
- **Docker Components**: Explore Docker Engine, images, containers, and more.
- **Project Building Using Docker**: Containerize a sample project.
- **Multi-stage Docker Builds / Distroless Images**: Optimize your images.
- **Docker Hub (Push/Tag/Pull)**: Manage and distribute your Docker images.
- **Docker Volumes**: Persist data across container runs.
- **Docker Networking**: Connect containers using networks.
- **Docker Compose**: Orchestrate multi-container applications.
- **Docker Scout**: Analyze your images for vulnerabilities and insights.

Complete all the tasks below and document your steps, commands, and observations in a file named solution.md. Finally, share your experience on LinkedIn using the provided guidelines.

---

## ðŸš€Task 1: Introduction and Conceptual Understanding


### 1ï¸âƒ£ ** Write an Introduction:
- In your solution.md, provide a brief explanation of Dockerâ€™s purpose in modern DevOps.

```
Docker is a tool that helps developers package, share, and run applications easily and consistently across different computers.

ðŸ”¹ Why is Docker Important in DevOps?
Works Everywhere â€“ Docker puts an app and everything it needs (code, dependencies, settings) inside a container. This container runs the same way on any system (Windows, Mac, Linux, cloud servers).

Saves Time â€“ No more "it works on my machine" issues! Developers can quickly set up environments without installing everything manually.

Fast & Lightweight â€“ Containers are smaller and faster than traditional virtual machines, making development and deployment much quicker.

Easier Collaboration â€“ Teams can share Docker containers, ensuring everyone works with the same setup.

Better Deployment â€“ Docker allows for automated deployments, making it easier to update and scale applications with tools like Kubernetes.

ðŸ’¡ Example:
Imagine you're building a website. Instead of setting up servers manually, you can run a Docker container with everything pre-configured and deploy it instantly.

ðŸŽ¯ Summary
Docker makes software development and deployment faster, more reliable, and efficientâ€”a perfect fit for DevOps! ðŸš€
```
- Compare Virtualization vs. Containerization and explain why containerization is the preferred approach for microservices and CI/CD pipelines.

|Feature Virtualization (VMs) | Containerization (Docker, Kubernetes)|
|-----------------------------|--------------------------------------|
|Architecture   | Uses a hypervisor to create multiple virtual machines, each with its own OS. |   Uses a container engine (like Docker) to run multiple lightweight containers on the same OS.|
|Resource Usage |  Heavy â€“ Each VM has a full OS, consuming more CPU, RAM, and storage. | Lightweight â€“ Containers share the same OS kernel, using fewer resources.|
|Startup Time    Slower â€“ Booting a VM takes minutes.  |  Fast â€“ Containers start in seconds.|
|Isolation |  Strong â€“ Each VM runs a full OS, fully isolated. |   Moderate â€“ Containers share the OS but are isolated at the application level.|
|Portability | Less portable â€“ VMs are large and require specific hypervisor setups. |  Highly portable â€“ Containers run the same way on any system (Linux, Windows, Mac, Cloud).|
|Scalability | Less efficient â€“ Requires more resources for scaling. |  Highly efficient â€“ Containers can be started, stopped, and scaled instantly.|


### Task 2ï¸âƒ£ ****Create a Dockerfile for a Sample Project

1. Select or Create a Sample Application:

- Choose a simple application (for example, a basic Node.js, Python, or Java app that prints â€œHello, Docker!â€ or serves a simple web page).

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
```
docker build -t <your-username>/sample-app:latest .
```
- Verify Your Build:
- Run your container locally to ensure it works as expected:
```
docker run -d -p 8080:80 <your-username>/sample-app:latest
```
- Verify the container is running with:
```
docker ps
```
- Check logs using:
```
docker logs <container_id>
```
![pic](https://github.com/user-attachments/assets/94660dc8-5589-40a8-a10b-fc1fedb47227)
![pic2](https://github.com/user-attachments/assets/ec801d2b-558c-4a69-9647-11846dd8d9c3)


## ðŸ—ï¸ Task 3: Explore Docker Terminologies and Components
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
``` 
docker images
```
![pic3 (2)](https://github.com/user-attachments/assets/46c9dbfe-4c11-473a-a548-ae40f439dffa)
![pic4](https://github.com/user-attachments/assets/9ad158d5-f2b9-4024-b1f6-c6af80b80352)

3. Document the Differences:
- Explain in solution.md the benefits of multi-stage builds and the impact on image size.
```
A multi-stage build is a Docker feature that allows you to use multiple FROM statements in a Dockerfile, where each stage produces an intermediate container, and only the final stage is included in the final image.
ðŸ”¹ Multi-stage builds significantly reduce image size â†’ Faster deployment & lower storage costs.
ðŸ”¹ Improved security by removing unnecessary dependencies.
ðŸ”¹ Faster CI/CD pipelines due to smaller image pull/push times.
ðŸ”¹ More maintainable Dockerfiles with clear separation of build and runtime stages.
```

## ðŸ”— Task 5: Manage Your Image with Docker Hub
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
![pic5](https://github.com/user-attachments/assets/6f8bbd78-10c8-43ba-b4fc-920315e5db4a)
![pic6](https://github.com/user-attachments/assets/31c2e74b-4e3e-4341-a7cb-bc15f94e1e05)

---

## ðŸ” Task 6: Persist Data with Docker Volumes
1. Create a Docker Volume:
- Create a Docker volume:
```
docker volume create my_volume
```
2. Run a Container with the Volume:
- Run a container using the volume to persist data:
```
docker run -d -v my_volume:/app/data <your-username>/sample-app:v1.0
```
![pic7](https://github.com/user-attachments/assets/69911a2c-bcd0-4bad-b0e7-01f2da925271)

3. Document the Process:
- In solution.md, explain how Docker volumes help with data persistence and why they are useful.
```
âœ” Persistent Data: Ensures important files survive container restarts & removals.
âœ” Efficient Data Sharing: Enables communication between multiple containers.
âœ” Better Performance: Optimized for Docker-managed storage.
âœ” Simplified Backups: Makes data management and recovery easier.
```

---

## ðŸŒ¿ Task 7: Configure Docker Networking
1. Create a Custom Docker Network:
- Create a custom Docker network:
```
docker network create my_network
```
2. Run Containers on the Same Network:
- Run two containers (e.g., your sample app and a simple database like MySQL) on the same network to demonstrate inter-container communication:
```
docker run -d --name sample-app --network my_network <your-username>/sample-app:v1.0
docker run -d --name my-db --network my_network -e MYSQL_ROOT_PASSWORD=root mysql:latest
```
![pic9](https://github.com/user-attachments/assets/e9971e4d-4dac-4a19-aeac-2ad17852207a)
3. Document the Process:
- In solution.md, describe how Docker networking enables container communication and its significance in multi-container applications.
```
âœ” Seamless Communication: Containers talk to each other within a defined network.
âœ” Security & Isolation: Different networks prevent unauthorized access.
âœ” Scalability: Works across multiple hosts in Swarm/Kubernetes.
âœ” Flexibility: Supports different network types for different use cases.
```

---

## ðŸ”’ Task 8: Orchestrate with Docker Compose
1. Create a docker-compose.yml File:
- Write a docker-compose.yml file that defines at least two services (e.g., your sample app and a database).
- Include definitions for services, networks, and volumes.
```
version: "3.8"

services:
  app:
    build: .
    container_name: python-app
    ports:
      - "80:80"
    depends_on:
      - db
    environment:
      - DATABASE_HOST=db
      - DATABASE_USER=root
      - DATABASE_PASSWORD=rootpassword
      - DATABASE_NAME=mydatabase
    networks:
      - my_network

  db:
    image: mysql:5.7
    container_name: mysql-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: user
      MYSQL_PASSWORD: userpassword
    ports:
      - "3306:3306"
    networks:
      - my_network
    volumes:
      - my_volume:/var/lib/mysql

networks:
  my_network:

volumes:
  my_volume:
```
2. Deploy Your Application:
- Bring up your application using:
```
docker-compose up -d
```
- Test the setup, then shut it down using:
```
docker-compose down
```
![pic10](https://github.com/user-attachments/assets/91ac6831-cf60-49b5-8cac-cbc638d63c48)
![pic11](https://github.com/user-attachments/assets/1cdb54ba-e785-44d2-971f-f51f403d2a6b)
![pic12](https://github.com/user-attachments/assets/f31b28be-a62b-4d95-a378-753eb574fd7f)
3. Document the Process:
- Explain each service and configuration in your solution.md
```
âœ”Specifies the Docker Compose file format version.
âœ”"3.8" is compatible with modern Docker versions.
âœ” build: . â†’ Builds the container using the Dockerfile in the same directory.
âœ” container_name: python-app â†’ Assigns a custom name to the container.
âœ” ports: â†’ Maps container port 80 to host port 80 for external access.
âœ” depends_on: â†’ Ensures MySQL (db) starts before the Python app.
âœ” environment: â†’ Sets environment variables for database connection.
âœ” networks: â†’ Connects to custom network (my_network) for container communication.
âœ” image: mysql:5.7 â†’ Uses MySQL 5.7 as the database engine.
âœ” container_name: mysql-db â†’ Assigns a name to the MySQL container.
âœ” restart: always â†’ Restarts the container automatically if it crashes.
âœ” environment: â†’ Sets up credentials and database configuration.
âœ” ports: â†’ Maps MySQL port 3306 for database connections.
âœ” networks: â†’ Connects to my_network for inter-container communication.
âœ” volumes: â†’ Uses a persistent volume (my_volume) to store database data.
âœ” Creates a user-defined network for secure container-to-container communication.
âœ” Both app and db services can talk to each other using service names (e.g., db).
âœ” my_volume ensures MySQL data is not lost when the container stops.
âœ” Stores database files inside /var/lib/mysql in a Docker-managed volume.
```
---

## ðŸŒ¿ Task 9: Analyze Your Image with Docker Scout
1. Run Docker Scout Analysis:

- Execute Docker Scout on your image to generate a detailed report of vulnerabilities and insights:
```
docker scout cves <your-username>/sample-app:v1.0
```
- Alternatively, if available, run:
```
docker scout quickview <your-username>/sample-app:v1.0
```
- to get a summarized view of the imageâ€™s security posture.
- Optional: Save the output to a file for further analysis:
```
docker scout cves <your-username>/sample-app:v1.0 > scout_report.txt
```
![scout](https://github.com/user-attachments/assets/10dcf139-965c-4c48-b6b3-9d8967575655)
![scout-1](https://github.com/user-attachments/assets/7ca3e8c3-b373-4970-a342-e9c018a50552)
![scout2](https://github.com/user-attachments/assets/99beda10-2994-438d-952f-7fa80293b180)
![scout3](https://github.com/user-attachments/assets/a05c115e-9f28-460c-a726-7f37c631bc90)
![scout4](https://github.com/user-attachments/assets/94bf6fcb-e35f-4a7a-902e-c3a8758ac7c4)
![scout5](https://github.com/user-attachments/assets/5d406335-00a7-4fd6-adb4-d1fc25ba959c)
![scout6](https://github.com/user-attachments/assets/d7ec2e51-4616-4d35-8196-89da51f0f347)
![scout7](https://github.com/user-attachments/assets/103e3866-1218-433b-92a4-e6fb19d21818)

2. Review and Interpret the Report:

- Carefully review the output and focus on:
- List of CVEs: Identify vulnerabilities along with their severity ratings (e.g., Critical, High, Medium, Low).
- Affected Layers/Dependencies: Determine which image layers or dependencies are responsible for the vulnerabilities.
- Suggested Remediations: Note any recommended fixes or mitigation strategies provided by Docker Scout.
- Comparison Step: If possible, compare this report with previous builds to assess improvements or regressions in your image's security posture.
- If Docker Scout is not available in your environment, document that fact and consider using an alternative vulnerability scanner (e.g., Trivy, Clair) for a comparative analysis.
3.  Document Your Findings:

- In your solution.md, provide a detailed summary of your analysis:
- List the identified vulnerabilities along with their severity levels.
- Specify which layers or dependencies contributed to these vulnerabilities.
- Outline any actionable recommendations or remediation steps.
- Reflect on how these insights might influence your image optimization or overall security strategy.
- Optional: Include screenshots or attach the saved report file (scout_report.txt) as evidence of your analysis.

## ðŸŒ¿ Task  10: Documentation and Critical Reflection
1. Update solution.md:
  - List all the commands and steps you executed.
  - Provide explanations for each task and detail any improvements made (e.g., image optimization with multi-stage builds).
2. Reflect on Dockerâ€™s Impact:
  - Write a brief reflection on the importance of Docker in modern software development, discussing its benefits and potential challenges.

Ÿ“¢ How to Submit
1. Push Your Final Work:
 - Ensure that your complete projectâ€”including your Dockerfile, docker-compose.yml, solution.md, and any additional files (e.g., the Docker Scout report if saved)â€”is committed and pushed to your repository.
 - Verify that all your changes are visible in your repository.
2. Create a Pull Request (PR):
 - Open a PR from your working branch (e.g., docker-challenge) to the main repository.
 - Use a clear and descriptive title, for example:
 - Week 5 Challenge - DevOps Batch 9: Docker Basics & Advanced Challenge
 - In the PR description, include the following details:
 - A brief summary of your approach and the tasks you completed.
 - A list of the key Docker commands used during the challenge.
 - Any insights or challenges you encountered (e.g., lessons learned from multi-stage builds or Docker Scout analysis).
3 Share Your Experience on LinkedIn:

 - Write a LinkedIn post summarizing your Week 5 Docker challenge experience.
  - In your post, include:
  - A brief description of the challenge and what you learned.
  - Screenshots, logs, or excerpts from your solution.md that highlight key steps or interesting findings (e.g., Docker Scout reports).
  - The hashtags: #90DaysOfDevOps #Docker #DevOps
  - Optionally, links to any blog posts or related GitHub repositories that further explain your journey.

~_~S Additional Resources
- Docker Documentation
- Docker Hub
- Multi-stage Builds
- Docker Compose
- Docker Scan (Vulnerability Scanning)
- Containerization vs. Virtualization

## Happy coding and best of luck with this Docker challenge! Document your journey thoroughly in solution.md and refer to these resources for additional guidance.

