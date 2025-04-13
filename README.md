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
