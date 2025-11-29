
# Running MySQL in Docker

In this session, we will learn:

1. How to run MySQL in a Docker container
2. How MySQL stores data inside containers
3. Why MySQL containers lose data when deleted
4. How to persist MySQL data using Docker volumes
5. How to inspect, connect, and interact with MySQL running in Docker

---

## 1. Introduction

MySQL is one of the most commonly used databases in Dockerized environments.

However, databases behave differently compared to simple applications:

* **They write data to disk**
* If the container is removed → **all data is lost**
* Databases therefore require **persistent storage**

This session explains how to properly run MySQL with persistent storage in Docker.

---

## 2. Running MySQL as a Container

The MySQL Docker image requires a **root password**, passed using environment variables.

### Basic run command

```bash
docker run --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -d mysql:8.0
```

### Explanation

| Part                         | Meaning                                          |
| ---------------------------- | ------------------------------------------------ |
| `--name mysql-db`            | Assigns a custom name to the container           |
| `-e MYSQL_ROOT_PASSWORD=...` | Sets the MySQL root password                     |
| `-d`                         | Runs the container in detached mode (background) |
| `mysql:8.0`                  | MySQL image and version                          |

---

## 3. Connecting to MySQL Inside the Container

### Method 1: Using `docker exec`

```bash
docker exec -it mysql-db mysql -u root -p
```

You will be prompted for the password you set earlier.

---

## 4. Where MySQL Stores Its Data

Inside the MySQL container, data is stored in:

```
/var/lib/mysql
```

This directory contains:

* Table data
* Indexes
* Database files
* Logs
* Metadata

If the container is deleted → **this entire folder disappears**.

---

## 5. Demonstration: What Happens When the Container Is Destroyed

### Step 1: Create a database

```sql
CREATE DATABASE demo;
```

### Step 2: Stop and remove the container

```bash
docker rm -f mysql-db
```

### Step 3: Start MySQL again

```bash
docker run --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
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

The `demo` database is gone.

### Conclusion

* Container filesystem = **temporary**
* Databases must use **persistent storage**

---

## 6. Solution: Use Docker Volumes

There are **two** ways to persist MySQL data:

### 1. Bind Mounts

Map a host folder → container folder.

### 2. Docker Volumes (Recommended)

Managed entirely by Docker and more reliable for databases.

We will cover both approaches in the next session.

