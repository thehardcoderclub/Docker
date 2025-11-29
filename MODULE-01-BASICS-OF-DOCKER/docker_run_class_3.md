

## 1. Running Specific Versions of Images (Tags)

When you run:

```bash
docker run redis
````

Docker pulls and runs the **latest** Redis image (e.g., `5.0.5` at the time of recording).

If you want to run a **different version**, specify a **tag**:

```bash
docker run redis:4.0
```

### Notes

* A **tag** identifies a specific version of an image.
* If no tag is provided → Docker defaults to the `latest` tag.
* Tags are defined by the image maintainers, not Docker.

### How to find available tags?

Visit Docker Hub → search for the image → view the **Tags** section.

Example:
Redis `5.0.5` may also have the tag `latest`.

---

## 2. Providing Input to a Container (Interactive Mode)

### The Problem

A simple CLI app that prompts for input works normally:

```bash
python app.py
# Enter Name:
```

But when containerized and run using:

```bash
docker run myapp
```

The app:

* prints output,
* **but cannot accept input**.

Why?
Because by default Docker does **not** attach standard input (stdin) and **does not create a terminal**.

---

## 3. Using `-i` and `-t` (Interactive + Terminal)

### `-i` (interactive)

Keeps **stdin open**, allowing you to provide input.

```bash
docker run -i myapp
```

### `-t` (pseudo-terminal)

Allocates a **virtual terminal**, making prompts visible.

```bash
docker run -t myapp
```

### Combine both (most common)

```bash
docker run -it myapp
```

This:

* enables input
* displays prompts
* provides a proper terminal experience

---

## 4. Port Mapping / Port Publishing

Consider a containerized web app listening on:

```
0.0.0.0:5000
```

### How does a user access it?

### Option 1 — Use the Container's IP

Every container receives an internal IP (example):

```
172.17.0.2:5000
```

But:

* This IP is **internal**
* Only accessible inside the Docker host

---

### Option 2 — Use the Docker Host's IP (Recommended)

To expose the container port externally:

```bash
docker run -p <host-port>:<container-port> image
```

Example:

```bash
docker run -p 80:5000 my-web-app
```

Now users can access the app at:

```
http://<docker-host-ip>:80
```

Example:

```
http://192.168.1.5:80
```

### Notes

* Only the host port must be unique.
* You can run multiple containers mapping different host ports.

Examples:

| Application | Command                         |
| ----------- | ------------------------------- |
| MySQL #1    | `docker run -p 3306:3306 mysql` |
| MySQL #2    | `docker run -p 8306:3306 mysql` |

You cannot map two containers to the same host port.

---

## 5. Persisting Data Using Volumes (`-v`)

If you run MySQL inside a container:

* Database files live inside the container at:
  `/var/lib/mysql`
* Deleting the container deletes all data.

### Solution: Map a host directory to a container directory.

Example:

```bash
mkdir -p /opt/data
docker run -v /opt/data:/var/lib/mysql mysql
```

Benefits:

* Data is stored **outside the container**
* Deleting the container does **not** remove the data
* This is called a **bind mount**

---

## 6. Inspecting Containers – `docker inspect`

To view detailed configuration info for a container:

```bash
docker inspect <container-id-or-name>
```

Outputs a JSON document containing:

* container state
* network settings
* mounted volumes
* environment variables
* command and entrypoint
* much more

Use this to debug configuration and networking issues.

---

## 7. Viewing Logs – `docker logs`

If a container runs in detached mode:

```bash
docker run -d my-web-app
```

You don’t see its output directly.

To view logs:

```bash
docker logs <container-id-or-name>
```

This outputs whatever the container writes to `stdout` or `stderr`.

---

## 8. Summary of New Options

| Feature                      | Option / Command                           |
| ---------------------------- | ------------------------------------------ |
| Use a specific image version | `docker run redis:4.0`                     |
| Interactive mode             | `docker run -i`                            |
| Allocate terminal            | `docker run -t`                            |
| Full interactive terminal    | `docker run -it`                           |
| Map ports                    | `docker run -p <host>:<container>`         |
| Create bind mount            | `docker run -v <host-dir>:<container-dir>` |
| Inspect container            | `docker inspect <id>`                      |
| View logs                    | `docker logs <id>`                         |

---

## 9. Conclusion

You now understand:

* How to run specific image versions
* How to use interactive mode and terminals
* How to publish ports
* How to persist data
* How to inspect containers
* How to view container logs

