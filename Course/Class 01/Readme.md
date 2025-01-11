# Class 1: Introduction and Fundamentals for Python Applications

Welcome to **Class 1** of our Docker course tailored for Python developers! In this session, you'll be introduced to the foundational concepts of Docker and containerization, specifically focusing on Python applications. By the end of this class, you'll have a solid understanding of what Docker is, its architecture, and how to set up your Docker environment to containerize Python applications effectively.

---

## Table of Contents

1. [Objectives](#objectives)
2. [Key Topics](#key-topics)
3. [Step-by-Step Guide](#step-by-step-guide)
4. [Hands-On Demonstration](#hands-on-demonstration)
5. [Resources](#resources)
6. [Assignment](#assignment)
7. [FAQs](#faqs)

---

## Objectives

By the end of this class, you will be able to:

- **Understand** the basic concepts of containerization and Docker.
- **Identify** the components of Docker architecture.
- **Install** Docker on your operating system.
- **Execute** essential Docker commands to manage containers and images.
- **Run** your first containerized Python application.

---

## Key Topics

### 1. Understanding Containerization

- **What are Containers?**
  - Lightweight, standalone, and executable packages that include everything needed to run a piece of software.
  
- **Containers vs. Virtual Machines**
  - Containers share the host system's OS, making them more efficient and faster to start compared to VMs, which emulate entire hardware systems.
  
- **Benefits of Using Docker**
  - Portability, consistency across environments, resource efficiency, and simplified deployment processes.

### 2. Docker Overview

- **History and Evolution of Docker**
  - Introduction to the inception of Docker and its impact on software development and deployment.
  
- **Key Components**
  - **Docker Engine:** The core application for building and running Docker containers.
  - **Docker Hub:** A cloud-based registry service for sharing Docker images.
  - **Docker CLI:** Command-line interface for interacting with Docker.

### 3. Docker Architecture

- **Docker Daemon (`dockerd`)**
  - Manages Docker objects like images, containers, networks, and volumes.
  
- **Docker Client (`docker`)**
  - The primary user interface for Docker, allowing users to interact with the Docker daemon via commands.
  
- **Docker Images and Containers**
  - **Images:** Read-only templates used to create containers.
  - **Containers:** Running instances of Docker images.
  
- **Registries**
  - Repositories where Docker images are stored and shared.

### 4. Setting Up Docker

- **Installing Docker**
  - **Windows:** Using Docker Desktop for Windows.
  - **macOS:** Using Docker Desktop for Mac.
  - **Linux:** Installation via package managers (e.g., `apt`, `yum`).
  
- **Configuring Docker Settings**
  - Adjusting resource allocation, network settings, and other preferences through Docker Desktop or configuration files.

### 5. Basic Docker Commands

- `docker run` - Run a container from an image.
- `docker ps` - List running containers.
- `docker stop` - Stop a running container.
- `docker rm` - Remove a stopped container.
- `docker images` - List Docker images.
- `docker pull` - Pull an image from a registry.
- `docker push` - Push an image to a registry.

---

## Step-by-Step Guide

### Step 1: Install Docker

#### **Step 1.1: Download Docker Desktop**

**Where to Run:** In your web browser.

1. **Download Docker Desktop:**
   - [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop)
   - [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop)

#### **Step 1.2: Install Docker Desktop**

**Where to Run:** On your local machine.

1. **Install Docker Desktop:**
   - Run the downloaded installer and follow the on-screen instructions.
   
2. **Launch Docker Desktop:**
   - After installation, launch Docker Desktop to ensure it's running correctly.

#### **Step 1.3: Verify Installation**

**Where to Run:** In your terminal or command prompt.

1. **Open Terminal/Command Prompt:**
   - **Windows:** Open Command Prompt or PowerShell.
   - **macOS/Linux:** Open Terminal.

2. **Check Docker Version:**
   ```bash
   docker --version
   ```
   - **Expected Output:**
     ```
     Docker version 20.10.7, build f0df350
     ```

### Step 2: Basic Docker Commands

#### **Step 2.1: Run the "Hello World" Container**

**Where to Run:** In your terminal or command prompt.

```bash
docker run hello-world
```

**Explanation:**
- This command downloads a test image and runs it in a container, displaying a confirmation message.

#### **Step 2.2: List Running Containers**

**Where to Run:** In your terminal or command prompt.

```bash
docker ps
```

**Explanation:**
- Displays all active containers.

#### **Step 2.3: List All Containers (Including Stopped)**

**Where to Run:** In your terminal or command prompt.

```bash
docker ps -a
```

**Explanation:**
- Lists all containers, regardless of their state.

#### **Step 2.4: Stop a Running Container**

**Where to Run:** In your terminal or command prompt.

```bash
docker stop <container_id>
```

**Explanation:**
- Stops a running container identified by `<container_id>`.

#### **Step 2.5: Remove a Stopped Container**

**Where to Run:** In your terminal or command prompt.

```bash
docker rm <container_id>
```

**Explanation:**
- Removes a stopped container identified by `<container_id>`.

#### **Step 2.6: List Docker Images**

**Where to Run:** In your terminal or command prompt.

```bash
docker images
```

**Explanation:**
- Lists all Docker images available locally.

#### **Step 2.7: Pull an Image from Docker Hub**

**Where to Run:** In your terminal or command prompt.

```bash
docker pull python:3.9-slim
```

**Explanation:**
- Downloads the `python:3.9-slim` image from Docker Hub.

#### **Step 2.8: Push an Image to Docker Hub**

**Where to Run:** In your terminal or command prompt.

```bash
docker push <your_dockerhub_username>/<image_name>
```

**Explanation:**
- Pushes the specified image to your Docker Hub repository.

---

## Hands-On Demonstration

### **Running the "Hello World" Container**

Follow these steps to verify your Docker installation by running the "Hello World" container.

#### **Step 1: Execute the Command**

**Where to Run:** In your terminal or command prompt.

```bash
docker run hello-world
```

#### **Step 2: Observe the Output**

**Where to View:** In your terminal or command prompt.

**Expected Output:**
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

#### **Step 3: Explore Docker Hub**

**Where to Access:** In your web browser.

1. **Visit Docker Hub:**
   - Navigate to [Docker Hub](https://hub.docker.com/) to browse available images.

2. **Pull a Public Image:**

   **Where to Run:** In your terminal or command prompt.

   ```bash
   docker pull nginx
   ```

3. **Run the Nginx Container:**

   **Where to Run:** In your terminal or command prompt.

   ```bash
   docker run -d -p 8080:80 --name my-nginx nginx
   ```

4. **Access Nginx in Your Browser:**

   **Where to Access:** Open your web browser.

   - Navigate to `http://localhost:8080` to see the Nginx welcome page.

5. **Manage the Nginx Container:**

   **Where to Run:** In your terminal or command prompt.

   - **List Running Containers:**
     ```bash
     docker ps
     ```
   
   - **Stop the Container:**
     ```bash
     docker stop my-nginx
     ```
   
   - **Remove the Container:**
     ```bash
     docker rm my-nginx
     ```

---

## Resources

- **Docker Official Documentation:** [https://docs.docker.com/](https://docs.docker.com/)
- **Docker Desktop Downloads:**
  - [Windows](https://www.docker.com/products/docker-desktop)
  - [macOS](https://www.docker.com/products/docker-desktop)
- **Introductory Video Tutorials:**
  - [Docker Getting Started](https://www.youtube.com/watch?v=fqMOX6JJhGo)
  - [Docker Tutorial for Beginners](https://www.youtube.com/watch?v=3c-iBn73dDE)
- **Python-Specific Resources:**
  - [Dockerizing Python Applications](https://docs.docker.com/samples/python/)
  - [Official Python Docker Images](https://hub.docker.com/_/python)

---

## Assignment

### **Objective:**

Apply what you've learned by setting up a Docker environment and running your first containerized Python application.

### **Tasks:**

1. **Install Docker** on your machine following the steps outlined in the [Step-by-Step Guide](#step-by-step-guide).

2. **Run the "Hello World" Container:**
   - **Where to Run:** In your terminal or command prompt.
   - Execute `docker run hello-world` and ensure you receive the confirmation message.

3. **Pull and Run an Nginx Container:**
   - **Where to Run:** In your terminal or command prompt.
   - Pull the Nginx image using `docker pull nginx`.
   - Run the Nginx container with port mapping:
     ```bash
     docker run -d -p 8080:80 --name my-nginx nginx
     ```
   - **Where to Access:** Open your web browser and navigate to `http://localhost:8080` to view the Nginx welcome page.

4. **Manage Your Containers:**
   - **Where to Run:** In your terminal or command prompt.
   - List all running containers using `docker ps`.
   - Stop and remove the Nginx container using:
     ```bash
     docker stop my-nginx
     docker rm my-nginx
     ```

5. **Document Your Setup:**
   - **Where to Run:** Use a text editor to create a Markdown (`.md`) or PDF report.
   - **What to Include:**
     - Step-by-step actions you performed.
     - Docker commands used and their outputs.
     - Screenshots of your terminal and browser (optional).
     - Any issues encountered and how you resolved them.

### **Submission:**

- Submit your report in PDF or Markdown format via the course platform by **[Due Date]**.

---

## FAQs

**Q1: What is the difference between Docker and Virtual Machines?**

**A:** Docker containers share the host system's OS, making them lightweight and faster to start compared to virtual machines, which emulate entire hardware systems and require separate operating systems.

---

**Q2: Do I need to create a Docker Hub account for this class?**

**A:** While not mandatory for Class 1, creating a Docker Hub account is recommended for future classes where you'll push and pull custom images.

---

**Q3: Can I install Docker on a virtual machine?**

**A:** Yes, Docker can be installed on a virtual machine. However, ensure that nested virtualization is supported and enabled if you're running Docker inside a VM.

---

**Q4: I'm facing issues installing Docker on my OS. What should I do?**

**A:** Refer to the [Docker Troubleshooting Guide](https://docs.docker.com/docker-for-windows/troubleshoot/) or seek assistance on the [Docker Community Forums](https://forums.docker.com/).

---

**Q5: Is it possible to run Docker without Docker Desktop on Windows or Mac?**

**A:** Docker Desktop provides an easy setup for Docker on Windows and Mac. While it's possible to use alternatives like Docker Toolbox or run Docker inside a Linux VM, Docker Desktop is the recommended approach for most users.

---

---