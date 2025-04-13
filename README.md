# Docker for Python Developers: Beginner to Advanced

## Module 1: Introduction to Docker 🚀

### 1.1 What is Docker?
Docker is a platform that allows developers to **build, ship, and run** applications inside lightweight, portable containers. These containers include everything needed to run the application, such as the code, runtime, libraries, and dependencies.  

### 1.2 Why Docker? (Problems It Solves)
Before Docker, developers faced challenges like:
- ❌ **"It works on my machine!"** – Apps behave differently on different systems.
- ❌ **Dependency Hell** – Conflicting package versions across projects.
- ❌ **Difficult Deployment** – Setting up environments manually is time-consuming.
- ❌ **Resource Heavy Virtual Machines (VMs)** – VMs take up too much space and boot slowly.

Docker solves these by:
- ✅ **Standardizing environments** – Containers behave the same across different systems.
- ✅ **Isolating dependencies** – Each app gets its own environment.
- ✅ **Quick deployment** – Running an app is as easy as `docker run`.
- ✅ **Lightweight & efficient** – Uses fewer resources than VMs.

### 1.3 Docker vs. Virtual Machines (VMs)
| Feature | Docker | Virtual Machine (VM) |
|---------|--------|---------------------|
| Boot Time | **Seconds** | Minutes |
| Resource Usage | **Lightweight** | Heavy (full OS per VM) |
| Isolation | **Process-level isolation** | Full OS-level isolation |
| Performance | **Faster** | Slower |
| Portability | **Runs anywhere** | OS-dependent |

Unlike VMs, which virtualize hardware, **Docker containers share the host OS kernel**, making them much more efficient.

### 1.4 Key Docker Concepts
- **Container** – A running instance of an application inside an isolated environment.  
- **Image** – A lightweight, standalone, and executable package that includes all dependencies.  
- **Dockerfile** – A script that defines how to build a Docker image.  
- **Volumes** – Persistent storage for data inside containers.  
- **Networks** – Allow communication between containers.

### 1.5 Installing Docker
You'll need to install **Docker Desktop** to get started. Follow the official guide for your OS:
- 🔹 [Windows](https://docs.docker.com/desktop/install/windows-install/)
- 🔹 [Mac](https://docs.docker.com/desktop/install/mac-install/)
- 🔹 [Linux](https://docs.docker.com/engine/install/)

After installation, verify using:
```bash
docker --version
```

### 1.6 Your First Docker Command 🚀
Run a simple container:
```bash
docker run hello-world
```
🔹 This pulls the `hello-world` image and runs it in a container.  
🔹 If successful, you’ll see a message confirming Docker is working.

---

## Module 2: Basic Docker Commands 🚀

### 2.1 Running a Container
```bash
docker run hello-world
```
Try running a Python container:
```bash
docker run python:3.9 python --version
```

### 2.2 Listing Running & Stopped Containers
```bash
docker ps       # Running only

docker ps -a    # All containers
```

### 2.3 Stopping & Removing Containers
```bash
docker stop <container_id>
docker rm <container_id>
docker container prune    # Remove all stopped containers
```

### 2.4 Pulling & Listing Images
```bash
docker pull ubuntu:latest
docker images
docker rmi <image_id>
```

### 2.5 Running an Interactive Shell Inside a Container
```bash
docker run -it ubuntu bash
docker run -it python:3.9 python
```
Use `exit` or `exit()` to quit the shell.

### 2.6 Checking Logs of a Container
```bash
docker logs <container_id>
docker logs -f <container_id>  # Real-time logs
```

### 2.7 Removing All Containers & Images
```bash
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
docker rmi $(docker images -q)
```

---

## Module 3: Docker Images & Dockerfile ⚙️

### 3.1 What is a Docker Image?
A **Docker image** is a snapshot of a file system and parameters needed to run an application. Images are used to **create containers**.

You can:
- Pull ready-made images from Docker Hub (e.g., Python, Ubuntu, Postgres)
- Build your **own custom image** with a Dockerfile

### 3.2 What is a Dockerfile?
A **Dockerfile** is a text file that contains a set of instructions to build a Docker image. Think of it like a recipe.

Example Dockerfile for a Python app:
```Dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

### 3.2.1 Line-by-Line Explanation
- `FROM python:3.9`: Specifies the base image. This pulls the official Python 3.9 image from Docker Hub.
- `WORKDIR /app`: Sets the working directory inside the container to `/app`. All subsequent commands will run from this directory.
- `COPY requirements.txt ./`: Copies `requirements.txt` from your host machine into the container's `/app` directory.
- `RUN pip install --no-cache-dir -r requirements.txt`: Installs Python dependencies listed in `requirements.txt`.
- `COPY . .`: Copies the rest of the app’s source code into the container.
- `CMD ["python", "main.py"]`: Sets the default command to run when the container starts.

### 3.3 Build an Image from Dockerfile
```bash
docker build -t my-python-app .
```
- `-t my-python-app`: Tags the image with the name `my-python-app`.
- `.`: Specifies the build context is the current directory (which includes the Dockerfile).

#### How it works internally:
1. Docker reads the Dockerfile.
2. Executes each instruction step by step in a new intermediate image layer.
3. Each layer is cached — so if nothing changes, builds are faster.
4. At the end, the final image is saved and can be used to run containers.

### 3.4 Run a Container from Your Image
```bash
docker run my-python-app
```
This runs a new container using the `my-python-app` image.

### 3.5 Docker Ignore File
Like `.gitignore`, a `.dockerignore` file prevents unnecessary files from being included in the image:
```text
__pycache__/
*.pyc
.env
```
This helps keep images smaller and cleaner.

### 3.6 Inspecting Images
```bash
docker images            # List all images

docker image inspect my-python-app  # Detailed metadata
```

### 3.7 Tagging and Versioning
Tag your image versions clearly:
```bash
docker build -t my-python-app:v1 .
docker run my-python-app:v1
```
This allows easier version control and rollback.

---

## Module 4: Dockerizing Python Applications 🐍

### Common Dockerfile Instructions You Should Know
Here are the most commonly used instructions in Python-related Dockerfiles:
- `FROM`: Sets the base image.
- `WORKDIR`: Sets the working directory inside the container.
- `COPY`: Copies files from your local machine to the container.
- `RUN`: Executes commands while building the image (like installing packages).
- `CMD`: Default command to run when a container starts (can be overridden).
- `ENTRYPOINT`: Like CMD, but can't be overridden unless explicitly specified. Use it when you want your container to behave like a CLI tool.
- `EXPOSE`: Documents the port the container app listens on (doesn't publish it).

We'll explore how and when to use these instructions through projects below.

> ⚡️ **Note**: Later in the course, we'll cover how to use Docker **volumes** to persist data and how to **map host folders** to container folders so files like outputs don't vanish after container exit. We'll also explain `-p` for publishing ports (e.g. `-p 5000:5000`) and how that makes container services accessible on your host machine.

---

### 4.1 Dockerizing a Flask App 🔥
#### Folder Structure
```
flask-app/
├── app.py
├── requirements.txt
└── Dockerfile
```

#### app.py
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Flask in Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

#### requirements.txt
```
flask
```

#### Dockerfile
```Dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### 4.2 Build and Run the Flask Container
```bash
docker build -t flask-app .
docker run -p 5000:5000 flask-app
```

Visit `http://localhost:5000` to test your app.

---

### 4.3 Dockerizing a FastAPI App ⚡

#### Folder Structure
```
fastapi-app/
├── main.py
├── requirements.txt
└── Dockerfile
```

#### main.py
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello from FastAPI in Docker!"}
```

#### requirements.txt
```
fastapi
uvicorn
```

#### Dockerfile
```Dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### 4.4 Run the FastAPI Container
```bash
docker build -t fastapi-app .
docker run -p 8000:8000 fastapi-app
```

Visit `http://localhost:8000` for the app. Check `/docs` for Swagger UI.

---

### 4.5 Exercise: Dockerizing a CLI Python Aggregator 📊

Create a Dockerized CLI app that:
- Reads a CSV file from a provided **input filename**
- Groups by a provided column (e.g. `city`, `category`, etc.)
- Aggregates using a specified function (e.g. `sum`, `mean`, `max`, `min`) on a numeric column (e.g. `sales`, `amount`, etc.)
- Writes the output to a **new file** specified by the user
- Uses **positional arguments** (not named args)

#### Sample command:
```bash
docker run -v $(pwd):/app cli-aggregator input.csv city sales sum output.csv
```

#### Requirements:
- Dockerfile
- main.py with logic for CSV read, group, aggregate, write
- `requirements.txt` if any

> Input and output file mapping will only work properly when using Docker volume mounts (`-v`), which we will explore in more depth in a future module.

---
