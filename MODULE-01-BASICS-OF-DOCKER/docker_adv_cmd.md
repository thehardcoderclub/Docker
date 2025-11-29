
# Docker Attached Mode, Detached Mode, Attach, and Inspect

### Advanced Docker Run Options – Complete Lecture Notes (Markdown)

---

## 1. Introduction

When starting containers using the `docker run` command, Docker provides multiple ways to manage how containers interact with your terminal and how you interact with them.

This session covers:

* What is **Attached Mode**
* What is **Detached Mode**
* How to **attach back** to running containers
* How to use **docker inspect** to view container details

These concepts are essential for running long-lived services, debugging issues, and understanding container behavior.

---

# 2. Attached Mode (Foreground)

Attached mode is the **default** behavior when you run a container.

### Example:

```
docker run ubuntu sleep 15
```

### What happens?

* The container runs in the **foreground**
* Your terminal is “attached” to the container’s main process
* You cannot use the terminal until the container exits
* Ctrl+C may interrupt or may not work depending on the process
* When the process finishes → container exits → terminal returns

### When attached mode is useful?

* Interactive shells (`docker run -it ubuntu bash`)
* Debugging applications
* Running commands that show output immediately
* Testing simple programs

### Example of attached interactive mode:

```
docker run -it ubuntu bash
```

This gives you a Linux shell *inside* the container.

---

# 3. Detached Mode (Background)

To run a container in the **background**, use the `-d` option:

```
docker run -d ubuntu sleep 1500
```

### What happens?

* Container starts in the background
* Terminal is immediately available
* Container continues running independently
* You can check it using:

```
docker ps
```

### When detached mode is useful?

* Running long-running services:

  * Web servers (Nginx, Apache)
  * Jenkins
  * Databases
  * Redis
  * Custom background apps
* Running background jobs or tasks
* Running multiple services together

Detached mode is the standard for production workloads.

---

# 4. Attaching Back to a Running Container

If a container is running in detached mode, you can **attach** back to its primary process.

### Command:

```
docker attach <container-id>
```

### What it does:

* Brings container output to your terminal
* Shows real-time logs/console output
* Makes your terminal foreground again

### Important Notes

* If you press **Ctrl+C** while attached, you may stop the container.
* If the container’s main process stops → container stops.
* Attach connects to `stdout`/`stderr`.

### Example workflow:

1. Start a container in background:

```
docker run -d ubuntu sleep 500
```

2. See running containers:

```
docker ps
```

3. Attach:

```
docker attach <container-id>
```

---

# 5. Detach vs Logs (When not to use attach)

If you only want to **view logs** of a running container but NOT attach to its main process:

```
docker logs -f <container-id>
```

* `logs -f` follows logs like `tail -f`
* It does **not** attach
* Ctrl+C **does not** stop the container

---

# 6. docker inspect – Deep Container Information

`docker inspect` shows a **complete JSON report** about a container or image.

### Command:

```
docker inspect <container-id>
```

### What information does inspect provide?

| Field             | Description                                |
| ----------------- | ------------------------------------------ |
| `Id`              | Unique container ID                        |
| `State`           | Running / stopped / restarting             |
| `Config`          | Entrypoint, command, environment variables |
| `NetworkSettings` | IP address, ports, gateway                 |
| `HostConfig`      | Port mappings, CPU/memory limits           |
| `Mounts`          | Volumes and bind mounts                    |
| `Image`           | Image used to create container             |
| `Created`         | Timestamp                                  |
| `Path`            | Main running command                       |

---

## 6.1 Finding the Container’s Internal IP Address

Inside the JSON output:

```
NetworkSettings → Networks → bridge → IPAddress
```

Example:

```
"IPAddress": "172.17.0.2"
```

This IP is **internal** to Docker’s bridge network.

---

## 6.2 Finding Mounted Volumes

Under:

```
"Mounts"
```

Example:

```
"Source": "/var/lib/docker/volumes/mysql-data/_data",
"Destination": "/var/lib/mysql",
```

This tells you where the host directory/volume is mapped inside the container.

---

## 6.3 Checking Environment Variables

Under:

```
Config → Env
```

Example:

```
"MYSQL_ROOT_PASSWORD=my-secret-pw"
```

---

## 6.4 Checking Port Mappings

Look under:

```
HostConfig → PortBindings
```

Example:

```
"8080/tcp": [
    {
        "HostPort": "8080"
    }
]
```

This means container port `8080` mapped to host port `8080`.

---

# 7. Differences Summary

### Attached Mode

* Default
* Terminal connected to container
* Good for debugging / interactive use
* Blocks terminal

### Detached Mode (`-d`)

* Runs in background
* Terminal is free
* Good for long-running services

### docker attach

* Reconnect terminal output to running container

### docker inspect

* Debugging and analyzing runtime details
* Find IP, ports, volumes, environment, state

---

# 8. Quick Cheat Sheet

```
# Run attached
docker run ubuntu sleep 10

# Run detached
docker run -d ubuntu sleep 200

# Attach back
docker attach <container-id>

# View logs (do not attach)
docker logs -f <container-id>

# Inspect container details
docker inspect <container-id>
```

---

# 9. Conclusion

You now understand:

* How Docker containers behave in the foreground and background
* How to attach and detach correctly
* How to debug containers using logs
* How to deeply inspect container configuration using `docker inspect`
