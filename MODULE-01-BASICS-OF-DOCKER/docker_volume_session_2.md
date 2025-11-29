
# Session: Docker Volumes – The Complete Guide

Docker volumes are one of the most important concepts when running stateful applications like MySQL, MongoDB, Redis, or any application that stores data.

This session explains:

* What volumes are
* How container storage works
* Why container data disappears
* Types of volumes
* How to use volumes with MySQL
* Why volumes are the recommended method for persistence

---

## 1. Why Do We Need Docker Volumes?

Containers are **ephemeral**.
This means:

* Their filesystem lives only as long as the container exists
* Deleting a container deletes all its internal files
* Databases lose all data if volumes are not used

To solve this, Docker uses **volumes**, which store data **outside** the container but are **mounted into** the container.

---

## 2. Understanding Container Filesystems

Each container has its own isolated filesystem.
When files are written inside a container, they go into Docker's internal storage (`overlay2`).

This filesystem is:

* **Temporary**
* **Deleted** when the container is removed
* **Not shared** between containers
* **Not visible** from the host machine

Therefore:

👉 If MySQL writes data to `/var/lib/mysql` inside the container,
👉 And the container is deleted,
👉 **ALL DATA IS LOST.**

---

## 3. What Is a Docker Volume?

A **volume** is a directory created and managed by Docker on the host machine.

Docker volumes:

* Live **outside** the container
* Are stored under `/var/lib/docker/volumes/` (Linux)
* Persist even if the container is deleted
* Can be reused by multiple containers
* Are the safest and easiest way to store data

Think of a Docker volume as an **external hard disk** for containers.

---

## 4. Types of Docker Storage

### 1. **Bind Mounts**

* You choose a **host directory**
* It maps to a **container directory**

Example:

```bash
docker run -v /host/mysql:/var/lib/mysql mysql
```

### 2. **Volumes (Managed by Docker)** → **RECOMMENDED**

* Docker decides where to store data
* Data is safe and isolated

Example:

```bash
docker volume create mysql-data
```

---

## 5. Creating and Using a Volume with MySQL

### Step 1: Create a volume

```bash
docker volume create mysql-data
```

### Step 2: Run MySQL with the volume attached

```bash
docker run --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -v mysql-data:/var/lib/mysql \
  -d mysql:8.0
```

#### Explanation:

| Component        | Meaning                                   |
| ---------------- | ----------------------------------------- |
| `mysql-data`     | Name of the volume                        |
| `/var/lib/mysql` | MySQL data directory inside the container |

---

## 6. Demonstration: Data Persistence with Volumes

### Step 1: Create a database

```sql
CREATE DATABASE demo;
```

### Step 2: Remove the container

```bash
docker rm -f mysql-db
```

### Step 3: Start a NEW container using the SAME volume

```bash
docker run --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -v mysql-data:/var/lib/mysql \
  -d mysql:8.0
```

### Step 4: Connect again

```bash
docker exec -it mysql-db mysql -u root -p
```

### Step 5: Check databases

```sql
SHOW DATABASES;
```

👉 The `demo` database **STILL EXISTS**.
👉 This proves the data lives in the volume, not the container.

---

## 7. Seeing the Volume Information

### List volumes:

```bash
docker volume ls
```

### Inspect volume:

```bash
docker volume inspect mysql-data
```

---

## 8. Removing Volumes

Volumes do **NOT** get removed when containers are deleted.

### To remove a specific volume:

```bash
docker volume rm mysql-data
```

### To remove all unused volumes:

```bash
docker volume prune
```

---

## 9. Summary

| Feature                         | No Volume | With Volume |
| ------------------------------- | --------- | ----------- |
| Data survives container removal | ❌ No      | ✅ Yes       |
| Recommended for databases       | ❌ No      | ✅ Yes       |
| Easy to backup & migrate        | ❌ No      | ✅ Yes       |
| Managed by Docker               | ❌ No      | ✅ Yes       |

### Key Rule:

**Never run database containers without volumes.**

---

## 10. Real-World Best Practices

* Always use **named volumes** with MySQL, PostgreSQL, Redis, etc.
* Avoid storing database data **inside containers**.
* Never map sensitive data using **bind mounts**.
* Volumes are the default in Docker Compose setups.

