# Class 2: Building and Managing Docker Images for Python Applications

Welcome to **Class 2** of our Docker course! In this session, you'll dive into the essentials of creating and managing Docker images tailored specifically for Python applications. You'll learn how to build custom Docker images using Dockerfiles, optimize them for efficiency, and manage them effectively. By the end of this class, you'll be equipped to containerize your Python applications, ensuring they are portable, consistent, and easy to deploy.

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

- **Create** custom Docker images for Python applications using Dockerfiles.
- **Understand** Docker image layers and best practices for image creation.
- **Optimize** Docker images for reduced size and improved performance.
- **Manage** Docker images by tagging, listing, and pushing to Docker Hub.
- **Run** containers from your custom Python images and interact with them effectively.

---

## Key Topics

### 1. Docker Images

- **Understanding Docker Image Layers**
  - How images are built in layers.
  - Benefits of layered architecture.
  
- **Pulling and Pushing Images to Docker Hub**
  - Retrieving images from registries.
  - Sharing images by pushing to Docker Hub.

### 2. Docker Containers

- **Creating, Starting, Stopping, and Removing Containers**
  - Lifecycle management of containers.
  
- **Container Lifecycle and Management**
  - Understanding the states of a container.
  - Best practices for container management.

### 3. Dockerfile Basics

- **Writing a Dockerfile for Python**
  - Key instructions: `FROM`, `RUN`, `CMD`, `EXPOSE`, `ENV`, `COPY`, `ADD`.
  
- **Building Custom Images Using Dockerfile**
  - Using `docker build` to create images.

### 4. Image Optimization

- **Best Practices for Writing Efficient Dockerfiles**
  - Minimizing the number of layers.
  - Using appropriate base images.
  
- **Minimizing Image Size and Improving Build Times**
  - Techniques to reduce image footprint.
  - Caching strategies during builds.

### 5. Hands-On Exercises

- **Creating and Running Custom Python Images**
  - Building a Dockerfile for a simple Python web application.
  - Running containers from custom images.
  
- **Optimizing Images and Pushing to Docker Hub**
  - Refining Dockerfiles for efficiency.
  - Sharing your images on Docker Hub.

---

## Step-by-Step Guide

### Step 1: Creating Your First Dockerfile for a Python Application

#### **Step 1.1: Choose a Base Image**

**Where to Run:** Use a text editor (e.g., VS Code, Sublime Text) to create and edit the Dockerfile.

Select an appropriate base image for your Python application. For example, to create a simple Flask application:

```dockerfile
FROM python:3.9-slim
```

#### **Step 1.2: Write a Simple Dockerfile**

**Where to Run:** In your project's root directory, create a file named `Dockerfile` using your text editor.

Create a file named `Dockerfile` in your project directory with the following content:

```dockerfile
# Use the official Python image as the base
FROM python:3.9-slim

# Set the working directory
WORKDIR /app

# Copy requirements.txt and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Expose the application port
EXPOSE 5000

# Define the command to run the application
CMD ["python", "app.py"]
```

#### **Step 1.3: Build the Docker Image**

**Where to Run:** Open your terminal or command prompt and navigate to your project directory.

Navigate to your project directory and build the image:

```bash
docker build -t my-python-app .
```

**Explanation:**

- `docker build` initiates the build process.
- `-t my-python-app` tags the image with the name `my-python-app`.
- `.` specifies the build context (current directory).

### Step 2: Running a Container from the Custom Image

#### **Step 2.1: Run the Container**

**Where to Run:** In your terminal or command prompt.

```bash
docker run -d -p 5000:5000 --name my-running-app my-python-app
```

**Explanation:**

- `-d` runs the container in detached mode.
- `-p 5000:5000` maps port 5000 of the host to port 5000 of the container.
- `--name my-running-app` names the container for easy reference.
- `my-python-app` is the image to run.

#### **Step 2.2: Verify the Container is Running**

**Where to Run:** In your terminal or command prompt.

```bash
docker ps
```

You should see `my-running-app` listed among the running containers.

#### **Step 2.3: Access the Application**

**Where to Access:** Open your web browser.

Open your browser and navigate to `http://localhost:5000` to see your running application.

### Step 3: Modifying the Dockerfile for Enhanced Python Application

#### **Step 3.1: Add EXPOSE Instruction**

**Where to Run:** In your text editor, update the `Dockerfile`.

Ensure your Dockerfile includes the `EXPOSE` directive to specify the port:

```dockerfile
EXPOSE 5000
```

#### **Step 3.2: Use COPY to Include Application Files**

**Where to Run:** In your text editor, update the `Dockerfile`.

Ensure all necessary application files are copied into the image:

```dockerfile
COPY . .
```

#### **Step 3.3: Update CMD to Start the Web Server**

**Where to Run:** In your text editor, update the `Dockerfile`.

Define the command to start your application:

```dockerfile
CMD ["python", "app.py"]
```

#### **Step 3.4: Rebuild and Run the Container**

**Where to Run:** In your terminal or command prompt.

Rebuild the image with the updated Dockerfile:

```bash
docker build -t my-python-app .
```

Run the updated container:

```bash
docker run -d -p 5000:5000 --name my-running-app my-python-app
```

### Step 4: Optimizing Docker Images for Python Applications

#### **Step 4.1: Implement Multi-Stage Builds**

**Where to Run:** In your text editor, update the `Dockerfile`.

Modify your Dockerfile to use multi-stage builds, reducing the final image size:

```dockerfile
# Stage 1: Build
FROM python:3.9-slim AS build

WORKDIR /app

# Install build dependencies
RUN apt-get update && apt-get install -y gcc

# Copy and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Stage 2: Production
FROM python:3.9-slim

WORKDIR /app

# Copy only the necessary files from the build stage
COPY --from=build /app /app

# Expose the application port
EXPOSE 5000

# Define the command to run the application
CMD ["python", "app.py"]
```

#### **Step 4.2: Use `.dockerignore` to Exclude Unnecessary Files**

**Where to Run:** In your project's root directory, create a file named `.dockerignore` using your text editor.

Create a `.dockerignore` file to exclude files and directories that are not needed in the Docker image:

```
__pycache__
*.pyc
*.pyo
*.pyd
.env
*.git
*.gitignore
Dockerfile
.dockerignore
```

#### **Step 4.3: Minimize the Number of Layers**

**Where to Run:** In your text editor, update the `Dockerfile`.

Combine multiple `RUN` instructions to reduce the number of layers:

```dockerfile
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*
```

### Step 5: Pushing to Docker Hub

#### **Step 5.1: Create a Docker Hub Account**

**Where to Run:** In your web browser.

If you haven't already, sign up for a free account on [Docker Hub](https://hub.docker.com/).

#### **Step 5.2: Tag Your Image Appropriately**

**Where to Run:** In your terminal or command prompt.

Replace `<your_dockerhub_username>` with your Docker Hub username:

```bash
docker tag my-python-app <your_dockerhub_username>/my-python-app
```

#### **Step 5.3: Push the Image to Docker Hub**

**Where to Run:** In your terminal or command prompt.

```bash
docker push <your_dockerhub_username>/my-python-app
```

**Note:** You may be prompted to log in to Docker Hub. Use `docker login` to authenticate if necessary.

---

## Hands-On Demonstration

### **Building and Running a Custom Python Flask Application**

Follow these steps to build, run, optimize, and push a Dockerized Python Flask application.

#### **Step 1: Set Up the Project Directory**

**Where to Run:** In your terminal or command prompt.

```bash
mkdir my-python-app
cd my-python-app
```

#### **Step 2: Initialize a Python Environment and Install Flask**

**Where to Run:** In your terminal or command prompt.

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
pip install Flask
```

#### **Step 3: Create `app.py`**

**Where to Run:** In your text editor, create a file named `app.py` in the `my-python-app` directory.

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Dockerized Python Flask App!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

#### **Step 4: Create `requirements.txt`**

**Where to Run:** In your terminal or command prompt.

```bash
pip freeze > requirements.txt
```

#### **Step 5: Create a `Dockerfile`**

**Where to Run:** In your text editor, create a file named `Dockerfile` in the `my-python-app` directory.

```dockerfile
# Use the official Python image as the base
FROM python:3.9-slim

# Set the working directory
WORKDIR /app

# Copy requirements.txt and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Expose the application port
EXPOSE 5000

# Define the command to run the application
CMD ["python", "app.py"]
```

#### **Step 6: Build the Docker Image**

**Where to Run:** In your terminal or command prompt, ensure you're in the `my-python-app` directory.

```bash
docker build -t my-python-app .
```

#### **Step 7: Run the Docker Container**

**Where to Run:** In your terminal or command prompt.

```bash
docker run -d -p 5000:5000 --name my-running-app my-python-app
```

#### **Step 8: Verify the Application**

**Where to Access:** Open your web browser.

Navigate to `http://localhost:5000` to see the message: **"Hello from Dockerized Python Flask App!"**

#### **Step 9: Optimize the Docker Image**

- **Implement Multi-Stage Builds:**

  **Where to Run:** In your text editor, update the `Dockerfile` as shown in [Step 4.1](#step-4-1-implement-multi-stage-builds).

- **Create `.dockerignore`:**

  **Where to Run:** In your terminal or command prompt, ensure you're in the `my-python-app` directory.

  ```bash
  echo "__pycache__
  *.pyc
  *.pyo
  *.pyd
  .env
  *.git
  *.gitignore
  Dockerfile
  .dockerignore" > .dockerignore
  ```

- **Rebuild the Optimized Image:**

  **Where to Run:** In your terminal or command prompt.

  ```bash
  docker build -t my-python-app .
  ```

#### **Step 10: Push the Image to Docker Hub**

**Where to Run:** In your terminal or command prompt.

```bash
docker tag my-python-app <your_dockerhub_username>/my-python-app
docker push <your_dockerhub_username>/my-python-app
```

---

## Resources

- **Dockerfile Best Practices:** [https://docs.docker.com/develop/develop-images/dockerfile_best-practices/](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- **Docker Official Documentation:** [https://docs.docker.com/](https://docs.docker.com/)
- **Docker Hub:** [https://hub.docker.com/](https://hub.docker.com/)
- **Sample Dockerfiles on GitHub:**
  - [Python Dockerfile Example](https://github.com/python/cpython/blob/main/Dockerfile)
  - [Flask Docker Example](https://github.com/pallets/flask/blob/main/examples/docker/Dockerfile)

---

## Assignment

### **Objective:**

Apply the concepts learned by building, optimizing, and managing a custom Docker image for a simple Python Flask application, and then push it to Docker Hub.

### **Tasks:**

1. **Set Up a Python Flask Project:**
   - **Where to Run:** In your terminal or command prompt.
   - Initialize a new Python project.
   - Install Flask or another Python web framework of your choice.
   - Create a simple web application that responds with a message (e.g., "Hello from Docker with Python!").

2. **Create a Dockerfile:**
   - **Where to Run:** In your text editor, create and edit the `Dockerfile`.
   - Write a Dockerfile that sets up the environment, installs dependencies, copies application code, exposes necessary ports, and defines the startup command.

3. **Build the Docker Image:**
   - **Where to Run:** In your terminal or command prompt.
   - Use `docker build` to create your custom image.
   - Verify the image is listed using `docker images`.

4. **Run a Container from the Image:**
   - **Where to Run:** In your terminal or command prompt.
   - Execute `docker run` to start a container from your image.
   - **Where to Access:** Open your web browser to access the application and ensure it's running correctly.

5. **Optimize the Docker Image:**
   - **Where to Run:** In your text editor and terminal or command prompt.
   - Implement multi-stage builds to reduce image size.
   - Create a `.dockerignore` file to exclude unnecessary files.
   - Combine `RUN` instructions to minimize the number of layers.

6. **Push the Image to Docker Hub:**
   - **Where to Run:** In your terminal or command prompt.
   - Tag your image with your Docker Hub username.
   - Push the image to your Docker Hub repository.

7. **Document Your Process:**
   - **Where to Run:** Use a text editor to create a Markdown (`.md`) or PDF report.
   - Detail each step you performed, including:
     - Dockerfile snippets.
     - Command outputs.
     - Any challenges faced and how you resolved them.

### **Submission:**

- Submit your report along with the link to your Docker Hub repository via the course platform by **[Due Date]**.

---

## FAQs

**Q1: What is the purpose of a Dockerfile?**

**A:** A Dockerfile is a script containing a series of instructions on how to build a Docker image. It defines the base image, installs dependencies, copies application code, sets environment variables, and specifies the command to run the application.

---

**Q2: How can I reduce the size of my Docker images?**

**A:** To minimize Docker image size:
- Use lightweight base images (e.g., `python:3.9-slim`).
- Implement multi-stage builds.
- Remove unnecessary files and dependencies.
- Combine `RUN` instructions to reduce layers.
- Use `.dockerignore` to exclude irrelevant files from the build context.

---

**Q3: Do I need a Docker Hub account to push images?**

**A:** Yes, you need a Docker Hub account to push images to the Docker Hub registry. You can sign up for a free account at [Docker Hub](https://hub.docker.com/).

---

**Q4: What is the difference between `COPY` and `ADD` in a Dockerfile?**

**A:** Both `COPY` and `ADD` are used to copy files from the host to the Docker image. However, `ADD` has additional features like extracting compressed files and fetching files from URLs. It's recommended to use `COPY` unless you specifically need the extra functionalities of `ADD`.

---

**Q5: How do I view the layers of a Docker image?**

**A:** You can inspect the layers of a Docker image using the `docker history` command:

```bash
docker history my-python-app
```

This command displays the size and instructions of each layer in the image.

---