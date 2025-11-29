
# Creating Your Own Docker Image

### Full Lecture Notes (Markdown)

---

## 1. Introduction

In this lecture, we’re going to learn **how to create your own Docker image**.

Before we dive into the process, let’s answer an important question:

### Why would you need to create your own image?

You typically build your own image when:

1. **You cannot find a required component** (service/software) on Docker Hub.
2. **Your team decides to containerize your application** for ease of deployment and portability.

In this example, we will containerize a **simple Python Flask web application**.

---

## 2. Understanding the Application

Before writing a Dockerfile, understand **how you would deploy the app manually**.
This helps you convert those steps into Dockerfile instructions.

### For a simple Flask app, the manual steps would be:

1. Start with an **operating system** like Ubuntu
2. Update package repositories using `apt`
3. Install required **system dependencies**
4. Install **Python dependencies** (`pip install -r requirements.txt`)
5. Copy application source code into the machine
6. Start the application using:

```
python app.py
```

These same steps will be converted into Dockerfile instructions.

---

## 3. The Image Creation Process

Creating your own image consists of 3 main steps:

### **Step 1: Write a Dockerfile**

* Define base OS
* Install packages
* Copy source files
* Set entrypoint / default command

### **Step 2: Build the Image**

```
docker build -t myapp:latest .
```

### **Step 3: Push the Image to Docker Hub**

```
docker push <dockerhub-username>/myapp
```

This makes your image publicly available.

---

# 4. Understanding the Dockerfile

A Dockerfile is a **plain text file** with a specific syntax used by Docker to automate image creation.

Each line has the format:

```
INSTRUCTION argument
```

For example:

```
FROM ubuntu
RUN apt-get update
COPY . /opt/source-code
ENTRYPOINT ["python", "app.py"]
```

Let’s break down each part.

---

## 4.1 FROM

```
FROM ubuntu
```

* Sets the **base image**
* All images *must* start with a `FROM` instruction
* The base is typically an OS or another image

Examples:

* `FROM ubuntu:20.04`
* `FROM python:3.9`
* `FROM node:18-alpine`

---

## 4.2 RUN

```
RUN apt-get update
RUN apt-get install -y python3-pip
```

* Executes commands inside the image at build time
* Used for installing dependencies

---

## 4.3 COPY

```
COPY . /opt/source-code
```

* Copies files from your host machine into the image
* Often used to copy:

  * application code
  * configuration files
  * requirements.txt

---

## 4.4 ENTRYPOINT

```
ENTRYPOINT ["python", "app.py"]
```

* Defines the command that executes **when a container is run**
* This is what “starts your app”

Examples:

```
ENTRYPOINT ["nginx", "-g", "daemon off;"]
ENTRYPOINT ["python", "server.py"]
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

# 5. Layered Architecture of Docker Images

Docker builds images using a **layered filesystem**.

Each Dockerfile instruction creates a **new layer**.

Example Dockerfile:

```
FROM ubuntu
RUN apt-get update
RUN apt-get install -y python3
COPY . /opt/app
ENTRYPOINT ["python3", "/opt/app/app.py"]
```

### The resulting layers:

1. Base Ubuntu image
2. Layer with `apt-get update` results
3. Layer with installed Python
4. Layer containing your app code
5. Layer defining entrypoint

### Why layers matter:

* Layers are **cached**
* Only changed layers and layers above them are rebuilt
* Rebuilds are faster
* Saves time and bandwidth
* Makes image distribution efficient

---

# 6. Docker Build Cache

One of Docker’s biggest advantages is **build caching**.

### Scenario:

Your build fails at Step 3 of your Dockerfile.

### After fixing the Dockerfile:

* Docker **reuses cached layers** for Step 1 and Step 2
* Only rebuilds from Step 3 onwards

This dramatically speeds up development.

### Example output during rebuild:

```
Step 1/5 : FROM ubuntu
 ---> Using cache
Step 2/5 : RUN apt-get update
 ---> Using cache
Step 3/5 : RUN pip install -r requirements.txt
 ---> Running...
```

---

# 7. Viewing Image History

To view layers of an existing image:

```
docker history myapp
```

This shows:

* Each instruction
* Each layer size
* Caching information

Useful for debugging bloated images.

---

# 8. Building the Image

Inside the folder containing your Dockerfile:

```
docker build -t myusername/myapp:1.0 .
```

Parameters:

| Part | Meaning                           |
| ---- | --------------------------------- |
| `-t` | Tag (name:version)                |
| `.`  | Build context (current directory) |

---

# 9. Pushing Image to Docker Hub

### Step 1: Login:

```
docker login
```

### Step 2: Push:

```
docker push myusername/myapp:1.0
```

Your image is now available publicly.

---

# 10. Key Takeaways

* Dockerfile **automates** the build steps of your application.
* Every instruction = a **layer**
* Layers are **cached**, making builds fast
* Use `FROM`, `RUN`, `COPY`, and `ENTRYPOINT` carefully
* Final image can be pushed to Docker Hub
* Containerized applications are portable, consistent, and easy to deploy

---

# 11. Quick Cheat Sheet

```
# Build image
docker build -t myapp .

# Run container
docker run myapp

# Push to Docker Hub
docker push username/myapp
