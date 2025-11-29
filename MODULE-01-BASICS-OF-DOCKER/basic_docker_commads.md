 
## Topic 2: Docker Basic Commands – Introduction & Usage

---

## 1. Introduction

In this part of the lecture, we explore essential Docker commands that you will use throughout your learning journey.  
At the end, you will complete a hands-on quiz to practice these commands directly in a lab environment.

---

## 2. The `docker run` Command

The **`docker run`** command is used to create and start a container from an image.

Example:

```bash
docker run nginx
````

* If the **nginx image exists locally**, Docker creates a container from it.
* If the image **does not exist locally**, Docker automatically pulls it from **Docker Hub**.
* The image is downloaded *only on the first run*; subsequent containers reuse the same image.

---

## 3. Viewing Running Containers – `docker ps`

To list **only running containers**, use:

```bash
docker ps
```

This displays:

* Container ID
* Image used
* Container status
* Container name (Docker assigns a random name like `silly_sammet`)

To view **all containers**, including exited ones:

```bash
docker ps -a
```

This shows both active and previously stopped containers.

---

## 4. Stopping a Container – `docker stop`

To stop a running container:

```bash
docker stop <container-id-or-name>
```

If you're unsure of the name, run:

```bash
docker ps
```

After stopping a container:

* `docker ps` → no running containers
* `docker ps -a` → shows the stopped container with status **Exited**

---

## 5. Removing a Container – `docker rm`

Stopped containers consume disk space.
To remove a container permanently:

```bash
docker rm <container-id-or-name>
```

Verify removal with:

```bash
docker ps -a
```

The container will no longer appear.

---

## 6. Viewing Images – `docker images`

To list all images stored locally:

```bash
docker images
```

This shows:

* Repository name
* Tags (explained later)
* Image ID
* Image size

Example output might include images like:

* nginx
* redis
* ubuntu
* alpine

---

## 7. Removing an Image – `docker rmi`

To delete an unused image:

```bash
docker rmi <image-id-or-name>
```

**Important:**
You must stop and delete all containers that depend on the image before removing it.

---

## 8. Pulling an Image Without Running It – `docker pull`

If you want to **download an image only**, without starting a container:

```bash
docker pull ubuntu
```

This pre-downloads the image so future containers start instantly.

---

## 9. Why Ubuntu Containers Exit Immediately

Running:

```bash
docker run ubuntu
```

creates a container **that exits instantly**. Why?

Containers are meant to run **a specific process**, not act as a full OS like virtual machines.

* Ubuntu image has *no default process*
* When no process runs, the container exits immediately

To keep it alive temporarily, run a command:

```bash
docker run ubuntu sleep 5
```

Container behavior:

* Runs `sleep 5`
* Sleeps for 5 seconds
* Exits when the command completes

---

## 10. Executing Commands in a Running Container – `docker exec`

If a container is running (example: sleeping for 100 seconds):

```bash
docker ps
```

To run a command inside it:

```bash
docker exec <container-id> cat /etc/hosts
```

This executes the command **inside** the container’s filesystem and prints the output.

---

## 11. Running Containers in Attached vs Detached Mode

### **Attached mode (default)**

Example:

```bash
docker run kodekloud/simple-webapp
```

* The container runs in the **foreground**
* You see logs/output directly
* Your terminal is "attached" to the container
* Press `CTRL + C` to stop the container
  → this also stops the application inside

### **Detached mode**

Run in the background:

```bash
docker run -d kodekloud/simple-webapp
```

* You immediately get your terminal back
* The container keeps running in the background
* Check status with:

```bash
docker ps
```

To reattach later:

```bash
docker attach <container-id>
```

**Shortcut tip:**
You can use just the first few characters of a container ID if they are unique.

---

## 12. Summary of Commands

| Purpose                          | Command                      |
| -------------------------------- | ---------------------------- |
| Run a container                  | `docker run <image>`         |
| Run in background                | `docker run -d <image>`      |
| List running containers          | `docker ps`                  |
| List all containers              | `docker ps -a`               |
| Stop a container                 | `docker stop <id>`           |
| Remove a container               | `docker rm <id>`             |
| List images                      | `docker images`              |
| Pull image only                  | `docker pull <image>`        |
| Remove image                     | `docker rmi <image>`         |
| Execute inside running container | `docker exec <id> <command>` |
| Attach to running container      | `docker attach <id>`         |

---

## 13. Next Steps

You will now move on to the **hands-on practice lab**, where you will:

* Run containers
* Stop/remove containers
* Pull and remove images
* Execute commands inside containers

This will help reinforce your understanding of the Docker CLI.

---

## Learning Reminders

Set up notifications, reminders, or calendar events to stay consistent with your learning path.


