
# Docker Basics – Hands-on Command Demo

Welcome to this demo!  
In this session, we will explore some of the **essential Docker commands** that you need to get started.  
In the previous demo, we installed Docker and ran sample containers such as:

- `hello-world`
- `ubuntu`

Now, we will dive deeper into commonly used Docker commands and learn how to run, inspect, stop, and remove containers and images.

---

## 1. Running Your First Container – `docker run`

The `docker run` command is used to run a container from an image.

Example:

```bash
docker run centos
````

### What happens?

1. Docker checks if the **centos** image exists locally.
2. If not, Docker pulls it from **Docker Hub**.
3. A container is created from that image.
4. Since no command is provided, the container **exits immediately**.

This is because base images like CentOS or Ubuntu do not run any default process unless specified.

---

## 2. Finding Images on Docker Hub

* Visit: **[https://hub.docker.com](https://hub.docker.com)**
* Browse official images under "Explore."
* Official images (e.g., `ubuntu`, `centos`, `nginx`, etc.) can be run using only the image name.

For user-created images, you must use the full naming format:

```
<dockerhub-username>/<repository-name>
```

Example:

```
mmsumshad/ansible-playable
```

---

## 3. Running a Container Interactively – `-it`

To run a container and get inside it:

```bash
docker run -it centos bash
```

Explanation:

* `-i` → interactive mode
* `-t` → allocate a pseudo-terminal
* `bash` → run the Bash shell inside the container

You will now be **inside** the container:

```
root@<container-id>:/#
```

Check the OS:

```bash
cat /etc/*release*
```

Exit container:

```bash
exit
```

---

## 4. Listing Containers – `docker ps`

To list **running containers**:

```bash
docker ps
```

To list **all containers**, including exited ones:

```bash
docker ps -a
```

### Example output fields:

* Container ID
* Image
* Command
* Status
* Container Name

Containers get automatically assigned a random name like `flamboyant_noyce`.

---

## 5. Keeping a Container Alive – Using `sleep`

Base images exit immediately unless you run a process.

Example:

```bash
docker run -d centos sleep 20
```

* `-d` runs the container in the background.
* Container stays alive for 20 seconds.

Check:

```bash
docker ps
```

After 20 seconds, it exits:

```bash
docker ps -a
```

---

## 6. Stopping a Running Container – `docker stop`

To stop a container:

```bash
docker stop <container-id-or-name>
```

Example:

```bash
docker stop serene_pasteur
```

If stopped manually, exit code will be:

* `137` → indicates a forced stop

---

## 7. Removing Containers – `docker rm`

Containers take disk space even after exiting.

Remove a container:

```bash
docker rm <container-id-or-name>
```

You can also remove **multiple containers**:

```bash
docker rm 345 e0a 773
```

You don't need full container IDs—just enough characters to uniquely identify them.

Check remaining containers:

```bash
docker ps -a
```

---

## 8. Listing Images – `docker images`

To view all downloaded images:

```bash
docker images
```

Example images:

* centos
* ubuntu
* busybox
* hello-world

BusyBox is a lightweight image (~1.13 MB).

---

## 9. Removing Images – `docker rmi`

Remove an image:

```bash
docker rmi busybox
```

Important:

> You **cannot remove an image** if any containers depend on it.

Example failure:

```bash
docker rmi centos
```

Output:
“You cannot remove this image since it is used by a container.”

Solution:

1. Remove the container(s):

   ```bash
   docker rm <container-id>
   ```

2. Then remove the image:

   ```bash
   docker rmi centos
   ```

---

## 10. Pulling Images Without Running Them – `docker pull`

If you want to download an image **without running it**:

```bash
docker pull ubuntu
```

Check:

```bash
docker images
```

---

## 11. Running Commands Inside a Container – `docker exec`

Steps:

1. Run a container:

   ```bash
   docker run -d ubuntu sleep 100
   ```

2. List running containers:

   ```bash
   docker ps
   ```

3. Run a command inside the container:

   ```bash
   docker exec <container-id> cat /etc/*release*
   ```

`docker exec` allows you to interact with a running container without logging into it.

---

## 12. Summary of Commands Used

| Action                                   | Command                       |
| ---------------------------------------- | ----------------------------- |
| Run a container                          | `docker run <image>`          |
| Run in background                        | `docker run -d <image>`       |
| Run interactively                        | `docker run -it <image> bash` |
| Show running containers                  | `docker ps`                   |
| Show all containers                      | `docker ps -a`                |
| Stop a container                         | `docker stop <id>`            |
| Remove container                         | `docker rm <id>`              |
| List images                              | `docker images`               |
| Remove image                             | `docker rmi <image>`          |
| Pull image only                          | `docker pull <image>`         |
| Execute command inside running container | `docker exec <id> <command>`  |

---

## 13. Conclusion

In this demo, we explored:

* Running containers
* Viewing running and stopped containers
* Stopping and removing containers
* Managing images
* Pulling images
* Executing commands inside containers

