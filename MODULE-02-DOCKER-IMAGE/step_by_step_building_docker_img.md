
# 📄 **docker-build-custom-image-session.md**

### *Creating Your Own Docker Image for a Simple Flask Application*

````md
# Session: Creating Your Own Docker Image (Step-by-Step Demo)

Welcome to this demo!  
In this session, we will learn how to **create our own Docker image** for a simple Python Flask application.

We will cover:

1. Understanding the application  
2. Running it manually inside a temporary Ubuntu container  
3. Documenting the installation steps  
4. Creating a Dockerfile  
5. Building a Docker image  
6. Running the custom image  
7. Pushing the image to Docker Hub  

---

## 1. Understanding the Application

We have a simple Python Flask web application hosted on GitHub.  
The app:

- Prints a welcome message on `/`
- Responds to `/howareyou` with “I’m good, how about you?”
- Runs on Flask’s default port: **5000**

The entire app is contained in a **single file `app.py`**.

Example:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Welcome!"

@app.route('/howareyou')
def how_are_you():
    return "I am good. How about you?"
````

To run this manually:

1. Install Python
2. Install Flask (`pip install flask`)
3. Run it with:

```bash
flask run --host=0.0.0.0
```

---

## 2. Running the App Manually Inside a Docker Container

We want to avoid installing dependencies on the Docker host.
Instead, we start a clean Ubuntu container and work inside it.

Run Ubuntu interactively:

```bash
docker run -it ubuntu bash
```

### Inside the container:

#### Step 1 — Update packages

```bash
apt-get update
```

#### Step 2 — Install Python

```bash
apt-get install -y python
```

Check version:

```bash
python --version
```

#### Step 3 — Install pip

```bash
apt-get install -y python-pip
```

#### Step 4 — Install Flask

```bash
pip install flask
```

#### Step 5 — Copy your application code

Create the file:

```bash
nano /opt/app.py
```

Paste the Flask application code here.

#### Step 6 — Run the application

```bash
cd /opt
flask run --host=0.0.0.0
```

The container is now serving your application internally.

You can access it using the container IP:

```
http://172.17.0.2:5000/
```

---

## 3. Documenting Installation Steps

Before creating a Dockerfile, always **write down the exact steps** used to set up the app manually.

Our manual steps were:

```
apt-get update
apt-get install -y python python-pip
pip install flask
copy app.py to /opt/app.py
run: flask run --host=0.0.0.0
```

We will convert these into Dockerfile instructions.

---

## 4. Creating the Dockerfile

Create a project directory:

```bash
mkdir my-simple-webapp
cd my-simple-webapp
```

Create a `Dockerfile`:

```bash
nano Dockerfile
```

### Dockerfile content:

```dockerfile
# 1. Base image
FROM ubuntu

# 2. Install dependencies
RUN apt-get update && \
    apt-get install -y python python-pip

# 3. Install Python packages
RUN pip install flask

# 4. Copy application source code
COPY app.py /opt/app.py

# 5. Run the application
ENTRYPOINT ["flask", "run", "--host=0.0.0.0"]
```

Next, place your `app.py` in the same directory:

```bash
nano app.py
```

Paste the Flask application code.

---

## 5. Building the Docker Image

Build the image with a tag:

```bash
docker build -t my-simple-webapp .
```

Docker follows each instruction → builds layers → caches them.

---

## 6. Running the Custom Image

Run it normally:

```bash
docker run my-simple-webapp
```

OR map port 5000 to the host so you can access it outside:

```bash
docker run -p 5000:5000 my-simple-webapp
```

Now open:

```
http://localhost:5000
```

You should see:

```
Welcome!
```

And:

```
http://localhost:5000/howareyou
```

---

## 7. Pushing the Image to Docker Hub

### Step 1 — Login

```bash
docker login
```

Enter Docker Hub username & password.

### Step 2 — Tag the image with your Docker Hub username

```bash
docker tag my-simple-webapp <your-username>/my-simple-webapp
```

Example:

```bash
docker tag my-simple-webapp mmumshad/my-simple-webapp
```

### Step 3 — Push the image

```bash
docker push <your-username>/my-simple-webapp
```

Example:

```bash
docker push mmumshad/my-simple-webapp
```

The image is now visible on your Docker Hub dashboard and can be pulled by anyone:

```bash
docker pull <your-username>/my-simple-webapp
```

---

## 8. Summary

In this demo we:

* Ran Ubuntu interactively
* Installed Python and Flask inside a temp container
* Documented installation steps
* Created a Dockerfile
* Built a custom Docker image
* Ran the image
* Published it on Docker Hub

You now know how to **containerize any application** by:

1. Running it manually once
2. Recording the steps
3. Translating those steps into a Dockerfile

This is the foundation of Docker image creation.

---

## Learning Reminder

Remember to practice by containerizing your own apps — nothing boosts your Docker skills faster!
