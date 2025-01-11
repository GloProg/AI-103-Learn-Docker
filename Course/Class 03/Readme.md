# Class 3: Docker Networking and Data Management for Python Applications

Welcome to **Class 3** of our Docker course tailored for Python developers! In this session, you'll learn how to configure Docker networking for inter-container communication and manage persistent data using Docker volumes and bind mounts. You'll apply these concepts to deploy a multi-container Python application, ensuring seamless communication between services and data persistence across container restarts. By the end of this class, you'll be able to design and manage complex Dockerized Python applications with multiple interconnected services.

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

- **Configure** Docker networks to enable communication between multiple containers.
- **Set Up** and manage Docker volumes and bind mounts for data persistence.
- **Deploy** a multi-container Python application using Docker networking and storage solutions.
- **Understand** best practices for networking and data management in Dockerized Python applications.

---

## Key Topics

### 1. Docker Networking Basics

- **Default Networks:**
  - **Bridge:** Default network for standalone containers.
  - **Host:** Removes network isolation between container and host.
  - **None:** Disables all networking.
  
- **Custom Networks:**
  - **Bridge Networks:** Enable communication between containers on the same host.
  - **Overlay Networks:** Enable communication between containers across multiple hosts (advanced).

### 2. Creating and Managing Docker Networks

- **Creating a Custom Bridge Network:**
  - `docker network create my-python-network`
  
- **Listing Networks:**
  - `docker network ls`
  
- **Inspecting a Network:**
  - `docker network inspect my-python-network`
  
- **Connecting Containers to Networks:**
  - Using the `--network` flag during container run.
  - `docker network connect my-python-network <container_name>`

### 3. Docker Storage Options

- **Volumes:**
  - Managed by Docker.
  - Persistent storage beyond container lifecycle.
  - Commands: `docker volume create`, `docker volume ls`, `docker volume inspect`, `docker volume rm`
  
- **Bind Mounts:**
  - Mounting a directory or file from the host into the container.
  - Useful for development and real-time data access.
  
- **tmpfs Mounts:**
  - Storage mounted directly into the host system’s memory.

### 4. Managing Persistent Data

- **Creating and Using Volumes:**
  - `docker volume create my-python-data`
  - Mounting a volume: `-v my-python-data:/data`
  
- **Using Bind Mounts for Development:**
  - Mounting local directories: `-v /path/to/local:/app`
  
- **Data Persistence Strategies:**
  - Ensuring data is not lost when containers are removed.
  - Backing up and restoring volumes.

### 5. Best Practices

- **Use Named Volumes for Data Persistence:**
  - Easier to manage and back up compared to anonymous volumes.
  
- **Isolate Networks for Different Services:**
  - Improves security and organization.
  
- **Avoid Using Host Networking:**
  - Maintains container portability and isolation.

---

## Step-by-Step Guide

### Step 1: Creating and Managing Docker Networks

#### **Step 1.1: Create a Custom Bridge Network**

**Where to Run:** In your terminal or command prompt.

```bash
docker network create my-python-network
```

**Explanation:**
- Creates a new bridge network named `my-python-network` for inter-container communication.

#### **Step 1.2: Verify the Network Creation**

**Where to Run:** In your terminal or command prompt.

```bash
docker network ls
```

**Expected Output:**
```
NETWORK ID     NAME                DRIVER    SCOPE
...
<network_id>   my-python-network   bridge    local
...
```

#### **Step 1.3: Inspect the Network**

**Where to Run:** In your terminal or command prompt.

```bash
docker network inspect my-python-network
```

**Explanation:**
- Provides detailed information about the network, including connected containers and configuration.

### Step 2: Setting Up Docker Volumes for Data Persistence

#### **Step 2.1: Create a Docker Volume**

**Where to Run:** In your terminal or command prompt.

```bash
docker volume create my-python-data
```

**Explanation:**
- Creates a new volume named `my-python-data` for storing persistent data.

#### **Step 2.2: Verify the Volume Creation**

**Where to Run:** In your terminal or command prompt.

```bash
docker volume ls
```

**Expected Output:**
```
DRIVER    VOLUME NAME
local     my-python-data
...
```

### Step 3: Deploying a Multi-Container Python Application

We'll deploy a simple Python Flask application with a PostgreSQL database, demonstrating inter-container networking and data persistence.

#### **Step 3.1: Project Structure**

**Where to Run:** On your local machine using a text editor.

Create the following directory structure:

```
my-multi-container-app/
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── db/
│   └── init.sql
└── docker-compose.yml
```

#### **Step 3.2: Create the Flask Application**

**Where to Run:** In your text editor, create `app/app.py`.

```python
from flask import Flask
import psycopg2
import os

app = Flask(__name__)

def get_db_connection():
    conn = psycopg2.connect(
        host='db',
        database='mydatabase',
        user='postgres',
        password='postgres'
    )
    return conn

@app.route('/')
def hello():
    conn = get_db_connection()
    cur = conn.cursor()
    cur.execute('SELECT version();')
    db_version = cur.fetchone()
    cur.close()
    conn.close()
    return f'Hello from Dockerized Python Flask App! PostgreSQL version: {db_version}'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

#### **Step 3.3: Define Dependencies**

**Where to Run:** In your text editor, create `app/requirements.txt`.

```
Flask==2.0.1
psycopg2-binary==2.9.1
```

#### **Step 3.4: Create the Flask Dockerfile**

**Where to Run:** In your text editor, create `app/Dockerfile`.

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

#### **Step 3.5: Initialize the PostgreSQL Database**

**Where to Run:** In your text editor, create `db/init.sql`.

```sql
CREATE DATABASE mydatabase;
```

#### **Step 3.6: Create Docker Compose Configuration**

**Where to Run:** In your text editor, create `docker-compose.yml`.

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
    external: true

volumes:
  my-python-data:
    external: true
```

**Explanation:**
- Defines two services: `web` (Flask app) and `db` (PostgreSQL).
- Connects both services to the `my-python-network`.
- Uses the `my-python-data` volume for PostgreSQL data persistence.
- Initializes the database using `init.sql`.

### Step 4: Deploying the Multi-Container Application

#### **Step 4.1: Navigate to the Project Directory**

**Where to Run:** In your terminal or command prompt.

```bash
cd my-multi-container-app
```

#### **Step 4.2: Deploy Using Docker Compose**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose up -d
```

**Explanation:**
- Builds and starts the `web` and `db` services in detached mode.

#### **Step 4.3: Verify the Deployment**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose ps
```

**Expected Output:**
```
    Name                   Command               State           Ports         
--------------------------------------------------------------------------
my-multi-container-app_db_1   docker-entrypoint.sh postgres    Up      5432/tcp
my-multi-container-app_web_1  python app.py                     Up      0.0.0.0:5000->5000/tcp
```

#### **Step 4.4: Access the Flask Application**

**Where to Access:** Open your web browser.

- Navigate to `http://localhost:5000`
- **Expected Output:** `Hello from Dockerized Python Flask App! PostgreSQL version: PostgreSQL 13.x.x on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu x.x.x-xubuntu1) ...`

### Step 5: Managing Containers and Volumes

#### **Step 5.1: List All Containers**

**Where to Run:** In your terminal or command prompt.

```bash
docker ps -a
```

#### **Step 5.2: Stop the Containers**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose down
```

**Explanation:**
- Stops and removes the containers while preserving the volumes.

#### **Step 5.3: Remove the Volume (Optional)**

**Where to Run:** In your terminal or command prompt.

```bash
docker volume rm my-python-data
```

**Note:** Removing the volume deletes all persisted data.

---

## Hands-On Demonstration

### **Deploying a Multi-Container Python Flask Application with PostgreSQL**

Follow these steps to deploy a Python Flask application connected to a PostgreSQL database using Docker networking and volumes.

#### **Step 1: Create Project Directory Structure**

**Where to Run:** In your terminal or command prompt.

```bash
mkdir my-multi-container-app
cd my-multi-container-app
mkdir app db
```

#### **Step 2: Create the Flask Application**

**Where to Run:** In your text editor, create `app/app.py`.

```python
from flask import Flask
import psycopg2
import os

app = Flask(__name__)

def get_db_connection():
    conn = psycopg2.connect(
        host='db',
        database='mydatabase',
        user='postgres',
        password='postgres'
    )
    return conn

@app.route('/')
def hello():
    conn = get_db_connection()
    cur = conn.cursor()
    cur.execute('SELECT version();')
    db_version = cur.fetchone()
    cur.close()
    conn.close()
    return f'Hello from Dockerized Python Flask App! PostgreSQL version: {db_version}'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

#### **Step 3: Define Dependencies**

**Where to Run:** In your text editor, create `app/requirements.txt`.

```
Flask==2.0.1
psycopg2-binary==2.9.1
```

#### **Step 4: Create the Flask Dockerfile**

**Where to Run:** In your text editor, create `app/Dockerfile`.

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

#### **Step 5: Initialize the PostgreSQL Database**

**Where to Run:** In your text editor, create `db/init.sql`.

```sql
CREATE DATABASE mydatabase;
```

#### **Step 6: Create Docker Compose Configuration**

**Where to Run:** In your text editor, create `docker-compose.yml`.

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
    external: true

volumes:
  my-python-data:
    external: true
```

#### **Step 7: Create the Docker Network and Volume**

**Where to Run:** In your terminal or command prompt.

```bash
docker network create my-python-network
docker volume create my-python-data
```

#### **Step 8: Deploy the Application Using Docker Compose**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose up -d
```

#### **Step 9: Verify the Deployment**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose ps
```

**Expected Output:**
```
    Name                   Command               State           Ports         
--------------------------------------------------------------------------
my-multi-container-app_db_1   docker-entrypoint.sh postgres    Up      5432/tcp
my-multi-container-app_web_1  python app.py                     Up      0.0.0.0:5000->5000/tcp
```

#### **Step 10: Access the Flask Application**

**Where to Access:** Open your web browser.

- Navigate to `http://localhost:5000`
- **Expected Output:** `Hello from Dockerized Python Flask App! PostgreSQL version: PostgreSQL 13.x.x on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu x.x.x-xubuntu1) ...`

#### **Step 11: Manage the Containers and Volumes**

**Stop and Remove Containers:**

**Where to Run:** In your terminal or command prompt.

```bash
docker-compose down
```

**Explanation:**
- Stops and removes the `web` and `db` containers while preserving the `my-python-data` volume.

**Remove the Volume (Optional):**

**Where to Run:** In your terminal or command prompt.

```bash
docker volume rm my-python-data
```

**Note:** This action deletes all persisted data in the volume.

---

## Resources

- **Docker Networking Documentation:** [https://docs.docker.com/network/](https://docs.docker.com/network/)
- **Docker Storage Documentation:** [https://docs.docker.com/storage/](https://docs.docker.com/storage/)
- **Docker Compose Documentation:** [https://docs.docker.com/compose/](https://docs.docker.com/compose/)
- **Flask Docker Example:** [https://github.com/pallets/flask/blob/main/examples/docker/Dockerfile](https://github.com/pallets/flask/blob/main/examples/docker/Dockerfile)
- **PostgreSQL Docker Image:** [https://hub.docker.com/_/postgres](https://hub.docker.com/_/postgres)
- **Python Docker Images:** [https://hub.docker.com/_/python](https://hub.docker.com/_/python)

---

## Assignment

### **Objective:**

Apply the concepts learned by deploying a multi-container Python Flask application with a PostgreSQL database using Docker networking and data management techniques.

### **Tasks:**

1. **Set Up Project Directory:**
   - **Where to Run:** In your terminal or command prompt.
   - Create a new directory for your multi-container application.
   - Initialize the directory structure as shown in the [Step 3.1](#step-31-project-structure) of the [Step-by-Step Guide](#step-by-step-guide).

2. **Create the Flask Application:**
   - **Where to Run:** In your text editor.
   - Develop a simple Flask application that connects to a PostgreSQL database and displays database information.

3. **Define Dependencies:**
   - **Where to Run:** In your text editor.
   - Create a `requirements.txt` file listing all necessary Python packages.

4. **Create the Flask Dockerfile:**
   - **Where to Run:** In your text editor.
   - Write a `Dockerfile` that sets up the environment, installs dependencies, copies application code, exposes necessary ports, and defines the startup command.

5. **Initialize the PostgreSQL Database:**
   - **Where to Run:** In your text editor.
   - Create an `init.sql` file to initialize the PostgreSQL database.

6. **Create Docker Compose Configuration:**
   - **Where to Run:** In your text editor.
   - Write a `docker-compose.yml` file to define the `web` and `db` services, networking, and volumes.

7. **Create Docker Network and Volume:**
   - **Where to Run:** In your terminal or command prompt.
   - Execute commands to create a custom Docker network and volume as outlined in [Step 3.6](#step-36-create-docker-compose-configuration).

8. **Deploy the Application:**
   - **Where to Run:** In your terminal or command prompt.
   - Use Docker Compose to build and run the multi-container application.

9. **Verify the Deployment:**
   - **Where to Run:** In your terminal or command prompt and web browser.
   - Ensure both containers are running and the Flask application is accessible.

10. **Manage Containers and Volumes:**
    - **Where to Run:** In your terminal or command prompt.
    - Practice stopping, removing containers, and managing volumes.

11. **Document Your Process:**
    - **Where to Run:** Use a text editor to create a Markdown (`.md`) or PDF report.
    - **What to Include:**
      - Step-by-step actions performed.
      - Docker commands used and their outputs.
      - Configuration files (`Dockerfile`, `docker-compose.yml`, etc.).
      - Screenshots of your terminal and browser (optional).
      - Any issues encountered and how you resolved them.

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