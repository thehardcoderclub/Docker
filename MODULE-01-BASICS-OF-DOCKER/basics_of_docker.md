# Module 2 – DevOps  
## Topic 1: Introduction to Docker – Why We Need It & What It Solves

---

## 1. Introduction

Modern applications often use multiple components such as:

- A Node.js web server  
- A MongoDB database  
- A Redis messaging or caching service  
- Additional orchestration or monitoring tools  

Managing these manually creates compatibility problems and slows development.  
My introduction to Docker began while trying to manage such a complex tech stack in one of my previous projects.

---

## 2. The Problem Before Docker

### 2.1 Compatibility Issues (“Matrix from Hell”)

Each service required:

- A specific OS version  
- Certain libraries or dependencies  
- Matching versions for compatibility  

Upgrading one component often broke another.  
This messy web of dependency conflicts is commonly called **“The Matrix from Hell.”**

---

### 2.2 Developer Onboarding Challenges

New developers had to:

- Install the correct operating system  
- Configure dependencies manually  
- Run dozens of commands to set up the environment  

This created slow onboarding and environment inconsistencies.

---

### 2.3 Inconsistent Environments Across Teams

Developers used different operating systems:

- Windows  
- Linux  
- macOS  

Applications behaved differently on each OS, causing unexpected bugs and failures.  
Consistency was impossible.

---

## 3. How Docker Solved These Problems

Docker provides **lightweight, isolated environments** called containers.

With Docker:

- Each component runs in its **own container**  
- Each container has its **own dependencies**  
- All containers share the **same Linux kernel**  
- Developers only need to run:  
  ```bash
  docker run ...
