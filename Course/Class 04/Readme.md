# Class 4: Docker Compose and Basic Orchestration for Python Applications

Welcome to **Class 4** of our Docker course tailored for Python developers! In this session, you'll learn how to manage multi-container Python applications using Docker Compose. You'll define and configure services, networks, and volumes in a `docker-compose.yml` file, and perform basic orchestration tasks such as scaling services and managing dependencies. By the end of this class, you'll be able to streamline the deployment and management of complex Dockerized Python applications with multiple interconnected services.

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

- **Utilize** Docker Compose to manage multi-container Python applications.
- **Define** and configure services, networks, and volumes in a `docker-compose.yml` file.
- **Perform** basic orchestration tasks such as scaling services and managing service dependencies.
- **Deploy** and manage the lifecycle of multi-container applications efficiently.
- **Understand** best practices for structuring Docker Compose configurations for Python projects.

---

## Key Topics

### 1. Introduction to Docker Compose

- **Purpose and Benefits:**
  - Simplifies the management of multi-container applications.
  - Defines services, networks, and volumes in a single configuration file.
  - Facilitates reproducible environments and streamlined deployments.

- **Installation:**
  - Docker Compose is included with Docker Desktop for Windows and macOS.
  - For Linux, installation via package managers or downloading the binary.

### 2. Anatomy of a `docker-compose.yml` File

- **Version Specification:**
  - Defines the version of Docker Compose syntax being used.

- **Services:**
  - Defines each container as a service with its configuration.

- **Networks:**
  - Configures custom networks for inter-service communication.

- **Volumes:**
  - Manages persistent data storage across services.

### 3. Defining Services in Docker Compose

- **Building Services:**
  - Using `build` to create images from Dockerfiles.
  - Using `image` to specify pre-built images.

- **Configuration Options:**
  - `ports`: Mapping container ports to host ports.
  - `environment`: Setting environment variables.
  - `volumes`: Mounting volumes and bind mounts.
  - `depends_on`: Defining service dependencies.

### 4. Managing Networks and Volumes

- **Custom Networks:**
  - Creating and assigning services to specific networks.

- **Persistent Storage:**
  - Defining named volumes for data persistence.
  - Utilizing bind mounts for development workflows.

### 5. Basic Orchestration with Docker Compose

- **Scaling Services:**
  - Running multiple instances of a service for load balancing.

- **Managing Service Lifecycle:**
  - Starting, stopping, and restarting services.
  - Viewing service logs and status.

- **Updating Services:**
  - Rolling updates and applying configuration changes.

### 6. Best Practices

- **Organizing `docker-compose.yml`:**
  - Structuring configurations for readability and maintainability.

- **Environment Separation:**
  - Managing different environments (development, testing, production) with multiple Compose files or profiles.

- **Security Considerations:**
  - Managing sensitive data with environment variables and secrets.

---

## Step-by-Step Guide

### Step 1: Installing Docker Compose

#### **Step 1.1: Verify Docker Compose Installation**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose --version
```

**Expected Output:**

```
docker-compose version 1.29.2, build 5becea4c
```

*Note: Docker Compose comes bundled with Docker Desktop on Windows and macOS. For Linux users, if it's not installed, follow the [official installation guide](https://docs.docker.com/compose/install/).*

### Step 2: Understanding the Existing Multi-Container Application

In **Class 3**, you deployed a multi-container Python Flask application with a PostgreSQL database using Docker Compose. We'll build upon that setup.

### Step 3: Creating and Configuring `docker-compose.yml`

#### **Step 3.1: Navigate to Project Directory**

**Where to Run:** In your terminal or command prompt.

```bash
cd my-multi-container-app
```

#### **Step 3.2: Create or Update `docker-compose.yml`**

**Where to Run:** In your text editor, open or create `docker-compose.yml` in the `my-multi-container-app` directory.

```yaml
version: '3.8'

services:
  web:
    build: ./app
    ports:
      - "5000:5000"
    networks:
      - my-python-network
    depends_on:
      - db
    environment:
      - DATABASE_HOST=db
      - DATABASE_USER=postgres
      - DATABASE_PASSWORD=postgres
      - DATABASE_NAME=mydatabase
    volumes:
      - ./app:/app
      - my-python-data:/var/lib/postgresql/data

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: mydatabase
    volumes:
      - my-python-data:/var/lib/postgresql/data
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - my-python-network

networks:
  my-python-network:
    driver: bridge

volumes:
  my-python-data:
```

**Explanation:**

- **Services:**
  - `web`: The Flask application service.
    - `build`: Specifies the build context using the Dockerfile in the `./app` directory.
    - `ports`: Maps port 5000 of the host to port 5000 of the container.
    - `networks`: Connects to the `my-python-network`.
    - `depends_on`: Ensures `db` service starts before `web`.
    - `environment`: Sets environment variables for database connection.
    - `volumes`: Mounts the local `./app` directory to `/app` in the container for real-time code updates (useful for development).
  - `db`: The PostgreSQL database service.
    - `image`: Uses the official PostgreSQL 13 image.
    - `environment`: Sets database credentials and name.
    - `volumes`: 
      - `my-python-data`: Named volume for persistent data storage.
      - `./db/init.sql`: Initializes the database with the provided SQL script.
    - `networks`: Connects to the `my-python-network`.

- **Networks:**
  - `my-python-network`: A custom bridge network for inter-service communication.

- **Volumes:**
  - `my-python-data`: A named volume for persisting PostgreSQL data.

### Step 4: Building and Deploying with Docker Compose

#### **Step 4.1: Build and Start Services**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose up -d --build
```

**Explanation:**

- `up`: Creates and starts containers.
- `-d`: Runs containers in detached mode.
- `--build`: Builds images before starting containers.

#### **Step 4.2: Verify Running Services**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose ps
```

**Expected Output:**

```
       Name                     Command               State           Ports         
-----------------------------------------------------------------------------------
my-multi-container-app_db_1   docker-entrypoint.sh postgres    Up      5432/tcp
my-multi-container-app_web_1  python app.py                     Up      0.0.0.0:5000->5000/tcp
```

#### **Step 4.3: Access the Flask Application**

**Where to Access:** Open your web browser.

- Navigate to `http://localhost:5000`
- **Expected Output:** `Hello from Dockerized Python Flask App! PostgreSQL version: PostgreSQL 13.x.x on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu x.x.x-xubuntu1) ...`

### Step 5: Scaling Services

#### **Step 5.1: Scale the `web` Service**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose up -d --scale web=3
```

**Explanation:**

- Scales the `web` service to run three instances.
- Useful for load balancing and handling increased traffic.

#### **Step 5.2: Verify Scaling**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose ps
```

**Expected Output:**

```
       Name                      Command               State           Ports         
-----------------------------------------------------------------------------------
my-multi-container-app_db_1    docker-entrypoint.sh postgres    Up      5432/tcp
my-multi-container-app_web_1   python app.py                     Up      0.0.0.0:5000->5000/tcp
my-multi-container-app_web_2   python app.py                     Up      0.0.0.0:5001->5000/tcp
my-multi-container-app_web_3   python app.py                     Up      0.0.0.0:5002->5000/tcp
```

**Note:** Ports are dynamically assigned for additional instances.

### Step 6: Managing Service Dependencies

#### **Step 6.1: Define Dependencies in `docker-compose.yml`**

**Where to Run:** In your text editor, ensure the `depends_on` directive is correctly set.

```yaml
services:
  web:
    ...
    depends_on:
      - db
    ...
```

**Explanation:**

- Ensures that the `db` service starts before the `web` service.
- Manages service startup order.

### Step 7: Updating Services

#### **Step 7.1: Modify Service Configuration**

**Where to Run:** In your text editor, update `docker-compose.yml` as needed (e.g., change environment variables, update image versions).

#### **Step 7.2: Apply Configuration Changes**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose up -d
```

**Explanation:**

- Recreates containers with the updated configurations.
- Applies changes without downtime by recreating only the affected services.

#### **Step 7.3: Rolling Back Changes (If Needed)**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose down
docker-compose up -d
```

**Explanation:**

- Stops and removes containers.
- Starts containers again, reverting to previous configurations if changes are not saved.

---

## Hands-On Demonstration

### **Managing a Multi-Container Python Flask Application with Docker Compose**

Follow these steps to manage your multi-container Python Flask application using Docker Compose, including scaling and handling service dependencies.

#### **Step 1: Navigate to Project Directory**

**Where to Run:** In your terminal or command prompt.

```bash
cd my-multi-container-app
```

#### **Step 2: Review `docker-compose.yml`**

**Where to Run:** In your text editor, open `docker-compose.yml`.

Ensure the configuration includes service definitions, networks, and volumes as outlined in [Step 3.2: Create or Update `docker-compose.yml`](#step-32-create-or-update-docker-composeyml).

#### **Step 3: Build and Deploy Services**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose up -d --build
```

#### **Step 4: Verify Services are Running**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose ps
```

#### **Step 5: Scale the `web` Service**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose up -d --scale web=3
```

#### **Step 6: Verify Scaling**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose ps
```

#### **Step 7: Access Each Flask Instance**

**Where to Access:** Open your web browser.

- Navigate to `http://localhost:5000`, `http://localhost:5001`, and `http://localhost:5002`
- Each URL corresponds to a different instance of the Flask application.

#### **Step 8: Modify Environment Variables**

**Where to Run:** In your text editor, update `docker-compose.yml` to change environment variables for the `web` service.

```yaml
services:
  web:
    ...
    environment:
      - DATABASE_HOST=db
      - DATABASE_USER=postgres
      - DATABASE_PASSWORD=postgres
      - DATABASE_NAME=mydatabase
      - FLASK_ENV=development
    ...
```

#### **Step 9: Apply Configuration Changes**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose up -d
```

#### **Step 10: Roll Back Changes (If Needed)**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose down
docker-compose up -d
```

**Explanation:**

- Stops and removes all containers.
- Starts containers again with previous configurations.

---

## Resources

- **Docker Compose Documentation:** [https://docs.docker.com/compose/](https://docs.docker.com/compose/)
- **Docker Networking Documentation:** [https://docs.docker.com/network/](https://docs.docker.com/network/)
- **Docker Storage Documentation:** [https://docs.docker.com/storage/](https://docs.docker.com/storage/)
- **Official Python Docker Images:** [https://hub.docker.com/_/python](https://hub.docker.com/_/python)
- **Flask Docker Example:** [https://github.com/pallets/flask/blob/main/examples/docker/Dockerfile](https://github.com/pallets/flask/blob/main/examples/docker/Dockerfile)
- **PostgreSQL Docker Image:** [https://hub.docker.com/_/postgres](https://hub.docker.com/_/postgres)

---

## Assignment

### **Objective:**

Apply the concepts learned by managing a multi-container Python Flask application using Docker Compose. You'll define services, configure networking and volumes, scale services, and handle service dependencies.

### **Tasks:**

1. **Review and Update `docker-compose.yml`:**
   - **Where to Run:** In your text editor.
   - Ensure your `docker-compose.yml` correctly defines the `web` and `db` services, networks, and volumes as outlined in the [Step-by-Step Guide](#step-by-step-guide).

2. **Build and Deploy Services:**
   - **Where to Run:** In your terminal or command prompt.
   - Execute `docker-compose up -d --build` to build images and start services.

3. **Verify Service Deployment:**
   - **Where to Run:** In your terminal or command prompt.
   - Use `docker-compose ps` to ensure all services are running.

4. **Scale the `web` Service:**
   - **Where to Run:** In your terminal or command prompt.
   - Execute `docker-compose up -d --scale web=3` to run three instances of the Flask application.

5. **Access Each Flask Instance:**
   - **Where to Access:** Open your web browser.
   - Navigate to `http://localhost:5000`, `http://localhost:5001`, and `http://localhost:5002` to verify each instance is running.

6. **Modify and Apply Configuration Changes:**
   - **Where to Run:** In your text editor and terminal.
   - Update environment variables or other configurations in `docker-compose.yml`.
   - Apply changes by running `docker-compose up -d`.

7. **Manage Service Dependencies:**
   - **Where to Run:** In your `docker-compose.yml` and terminal.
   - Ensure `web` service depends on `db` service using the `depends_on` directive.
   - Restart services to apply dependencies.

8. **Document Your Process:**
   - **Where to Run:** Use a text editor to create a Markdown (`.md`) or PDF report.
   - **What to Include:**
     - Step-by-step actions performed.
     - Docker Compose commands used and their outputs.
     - Configuration files (`docker-compose.yml`, `Dockerfile`, etc.).
     - Screenshots of your terminal and browser (optional).
     - Any challenges encountered and how you resolved them.

### **Submission:**

- Submit your report along with the link to your Docker Hub repository (if applicable) via the course platform by **[Due Date]**.

---

## FAQs

**Q1: What is the difference between a bridge network and an overlay network in Docker?**

**A:** A bridge network is used for communication between containers on the same host, while an overlay network allows containers to communicate across multiple Docker hosts, facilitating multi-host networking scenarios.

---

**Q2: How do I connect an existing container to a new network?**

**A:** Use the `docker network connect` command:

```bash
docker network connect my-python-network <container_name>
```

---

**Q3: Can I use Docker volumes with Docker Compose?**

**A:** Yes, Docker Compose allows you to define and manage volumes within the `docker-compose.yml` file, making it easier to handle persistent data for your services.

---

**Q4: What are the benefits of using bind mounts over volumes?**

**A:** Bind mounts are useful for development environments where you need to reflect real-time changes in your code within the container. They allow you to mount specific directories or files from the host into the container.

---

**Q5: How can I back up data from a Docker volume?**

**A:** You can back up a Docker volume using the following command:

```bash
docker run --rm -v my-python-data:/data -v $(pwd):/backup busybox tar czf /backup/backup.tar.gz /data
```

This command creates a compressed archive of the volume's data in the current directory.

---