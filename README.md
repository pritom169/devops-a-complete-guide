# DevOps - A Complete Guide

Welcome to my DevOps documentation repository. It includes notes from system administration fundamentals to advanced Cloud Infrastructure and CI/CD pipelines.

The goal of this repository is to translate theoretical concepts into practical, production-ready documentation and scripts, demonstrating the "How" and "Why" behind DevOps tools.

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Bash](https://img.shields.io/badge/Shell_Scripting-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![DevOps](https://img.shields.io/badge/DevOps-Phase_2-blue?style=for-the-badge)

---

## Repository Structure

This repository is compartmentalized into learning modules. Click on a module title to view the detailed notes and scripts.

### 🐧 [Module 01: Linux Administration & Shell Scripting](./linux/README.md)\*\*

Linux is the foundation of DevOps. In this module, I have compiled comprehensive documentation covering the File Hierarchy Standard (FHS), user management, security hardening, and automation via Bash.

#### **Key Competencies Covered**

| Domain                  | Technical Concepts & Skills                                                                                                                                                                                                                                                                       |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **System Architecture** | **File System Hierarchy**: Deep dive into the purpose of `/etc`, `/var`, `/usr`, `/opt`, and `/boot`.<br>**Device Management**: Understanding hardware interaction via `/dev` and `/media` mounts.<br>**Kernel Interaction**: Managing kernel modules and libraries in `/lib`.                    |
| **Package Management**  | **Dependency Resolution**: How `apt` handles dependencies vs. `snap` self-contained packages.<br>**Repositories**: Managing official repos, PPAs, and local package indexing.<br>**Software Integrity**: Understanding package authenticity and updates.                                          |
| **Security & Access**   | **User Management**: Lifecycle of users/groups (`useradd`, `usermod`), UID/GID, and service accounts.<br>**Permissions Model**: Granular control using Octal (755, 644) and Symbolic modes (`chmod`, `chown`).<br>**SSH Hardening**: Public/Private key authentication flows and handshake logic. |
| **Automation (Bash)**   | **Scripting**: Shebang usage, variables, loops (`for`, `while`), and conditional logic (`if/else`).<br>**I/O Streams**: Piping (`\|`), redirection (`>`, `>>`), and filtering with `grep`.<br>**Environment**: Persistent configuration via `.bashrc` vs. `/etc/environment`.                      |
| **Core Tools**          | **Vim Editor**: Proficiency in Command vs. Insert modes, text manipulation, and buffers.<br>**Process Management**: Handling service accounts and background processes.                                                                                                                           |

**[Click here to read the detailed Linux Notes](./linux/README.md)**

---

### 🐳 [Module 02: Containers & Docker](./containers/README.md)

Containers revolutionized application deployment by packaging code with dependencies into portable, isolated units. This module covers Docker fundamentals, architecture, and production best practices.

#### **Topics Covered**

| Category | Topics | Key Highlights |
|:---|:---|:---|
| **Fundamentals** | • Container concepts<br>• Images vs Containers<br>• Docker vs VMs | Lightweight isolation, shared kernel, 10-100x efficiency vs VMs |
| **Architecture** | • Docker Client/Daemon<br>• Container Runtime<br>• Registries (Hub, ECR) | Client-server model, containerd/runc, image distribution |
| **Core Commands** | • Image management<br>• Container lifecycle<br>• Port mapping<br>• Networking | `pull`, `run`, `exec`, `logs`, `-p` flag, container DNS |
| **Dockerfile** | • Build instructions<br>• Layer optimization<br>• Multi-stage builds | `FROM`, `COPY`, `RUN`, `CMD`, caching strategies |
| **Docker Compose** | • Multi-container apps<br>• Service orchestration<br>• Volumes & networks | YAML configuration, `docker-compose up/down` |
| **Security** | • Image scanning<br>• Non-root users<br>• Secret management<br>• `.dockerignore` | Trivy/Scout, least privilege, BuildKit secrets |
| **Best Practices** | • Minimal base images<br>• Version pinning<br>• Size optimization | Alpine/distroless, avoid `latest`, layer reduction |
| **CI/CD Integration** | • AWS ECR workflow<br>• Image tagging<br>• Registry authentication | Build → Test → Push → Deploy pipeline |

**[Click here to read the detailed Container Notes](./containers/README.md)**

---

## Learning Roadmap

- **Phase 1:** Linux Fundamentals & Scripting
- **Phase 2:** Containerization (Docker)
- **Phase 3:** Container Orchestration (Kubernetes)
- **Phase 4:** CI/CD Pipelines (GitLab CI/CD)
- **Phase 5:** Infrastructure as Code (Terraform/Ansible)
- **Phase 6:** Monitoring (Prometheus, Grafana, ELK Stack)

---

## About This Project

If you find these notes helpful or have suggestions for optimization, feel free to open an issue or pull request.