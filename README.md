# DevOps - A Complete Guide

Welcome to my DevOps documentation repository. It includes notes from system administration fundamentals to advanced Cloud Infrastructure and CI/CD pipelines.

The goal of this repository is to translate theoretical concepts into practical, production-ready documentation and scripts, demonstrating the "How" and "Why" behind DevOps tools.

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Bash](https://img.shields.io/badge/Shell_Scripting-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)
![DevOps](https://img.shields.io/badge/DevOps-Phase_1-blue?style=for-the-badge)

---

## Repository Structure

This repository is compartmentalized into learning modules. Click on a module title to view the detailed notes and scripts.

### 🐧 [Module 01: Linux Administration & Shell Scripting](./linux/README.md)\*\*

Linux is the foundation of DevOps. In this module, I have compiled comprehensive documentation covering the File Hierarchy Standard (FHS), user management, security hardening, and automation via Bash.

#### **Key Competencies Covered**

| Domain                  | Technical Concepts & Skills                                                                                                                                                                                                                                                                       |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **System Architecture** | **File System Hierarchy**: Deep dive into the purpose of `/etc`, `/var`, `/usr`, `/opt`, and `/boot`.<br>**Device Management**: Understanding hardware interaction via `/dev` and `/media` mounts.<br>**Kernel Interaction**: Managing kernel modules and libraries in `/lib`.                    |
| **Package Management**  | **Dependency Resolution**: How `apt` handles dependencies vs. `snap` self-contained packages.<br>**Repositories**: Managing official repos, PPAs, and local package indexing.<br>**Software Integrity**: Understanding package authenticity and updates.                                          |
| **Security & Access**   | **User Management**: Lifecycle of users/groups (`useradd`, `usermod`), UID/GID, and service accounts.<br>**Permissions Model**: Granular control using Octal (755, 644) and Symbolic modes (`chmod`, `chown`).<br>**SSH Hardening**: Public/Private key authentication flows and handshake logic. |
| **Automation (Bash)**   | **Scripting**: Shebang usage, variables, loops (`for`, `while`), and conditional logic (`if/else`).<br>**I/O Streams**: Piping (`                                                                                                                                                                 | `), redirection (`>`, `>>`), and filtering with `grep`.<br>**Environment**: Persistent configuration via `.bashrc`vs.`/etc/environment`. |
| **Core Tools**          | **Vim Editor**: Proficiency in Command vs. Insert modes, text manipulation, and buffers.<br>**Process Management**: Handling service accounts and background processes.                                                                                                                           |                                                                                                                                          |

**[Click here to read the detailed Linux Notes](./linux/README.md)**

---

## Learning Roadmap

- [x] **Phase 1: Linux Fundamentals & Scripting** (Current Focus)
- [ ] **Phase 2:** Containerization (Docker)
- [ ] **Phase 3:** Container Orchestration (Kubernetes)
- [ ] **Phase 4:** CI/CD Pipelines (Jenkins/GitHub Actions)
- [ ] **Phase 5:** Infrastructure as Code (Terraform/Ansible)

---

## About This Project

If you find these notes helpful or have suggestions for optimization, feel free to open an issue or pull request.
