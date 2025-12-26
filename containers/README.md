# Containers
## What are containers?

Containers are a form of virtualization that packages up an application and its dependencies into a single, portable unit. 

## Where do containers live?
Containers lives in 
- Container Repository
- Private Repository
- Public Repository for Docker (Docker Hub)

## Software Development Workflow before and after containers

Deploying software has evolved significantly. To understand the "revolution" of containers, we have to look at the painful processes that came before them.

Here is a detailed breakdown of how deployment worked before containers, how it works now, and exactly why that shift occurred.

---

### 1. The "Before" Era: Bare Metal & Virtual Machines

Before containers became the standard (roughly pre-2013), teams deployed applications in two main ways: **Bare Metal** or **Virtual Machines (VMs)**.

#### Phase A: The "Bare Metal" Days

Developers wrote code, and "Operations" installed it directly onto a physical server in a rack.

* **The Process:** You had to manually install the Operating System (OS), then the web server, then the libraries, and finally the app.
* **The Problem:** If you wanted to run two apps on one server, and App A needed `Python 2.7` while App B needed `Python 3.0`, you were in trouble. This was called **"Dependency Hell."** To avoid this, companies often bought a separate physical server for every single application, which was incredibly expensive and wasteful.

#### Phase B: The Virtual Machine (VM) Era

To solve the waste of bare metal, we started using VMs. This allowed us to run multiple "virtual" computers on one big physical server.

* **The Process:** Developers would package their code (usually as a `.jar`, `.war`, or zip file) and send it to the SysAdmin. The SysAdmin would provision a VM (which had a full Guest OS), install the necessary dependencies, and deploy the code.
* **The Bottleneck:**
* **"It works on my machine":** A developer might use Windows with specific libraries installed. The production server uses Linux with slightly different library versions. The code crashes in production, leading to the developer saying, "I don't know why; it works on my machine."
* **Heavy & Slow:** Every VM required a full Operating System (e.g., a full install of Linux or Windows). This took up GBs of space and minutes to boot up.



---

### 2. The "After" Era: Container Deployment

Containers changed the unit of delivery. Instead of delivering just the *code*, developers now deliver the *entire environment*.

#### The New Process (The Docker Workflow)

1. **The Manifest (Dockerfile):** The developer writes a text file called a `Dockerfile`. This file lists the OS, libraries, and code needed.
* *Example:* "Start with Alpine Linux, install Python 3.9, copy my code files, and run this command."


2. **The Build:** The developer runs a build command. This creates a **Container Image**. This image is a sealed, read-only snapshot of the application *and* its environment.
3. **The Registry:** The developer pushes this image to a central "library" (like Docker Hub or AWS ECR).
4. **The Run:** The server pulls the image and runs it.

Because the image already contains the OS libraries and dependencies, the server doesn't need to have anything installed other than the Container Runtime (like Docker).

---

### 3. Key Benefits of Using Containers

The shift to containers brought five major benefits that solved the headaches of the VM era:

#### 1. Solves "It Works on My Machine" (Consistency)

This is the biggest benefit. Because the container includes the code *and* the specific versions of all libraries, it runs exactly the same way on a developer's laptop as it does on a massive production server. The environment is no longer a variable; it is immutable.

#### 2. Efficiency (Lightweight)

Unlike a VM, a container does not need a full Guest Operating System.

* **VM:** 1 Application = 1 App + **1 Full OS (GBs of size)**.
* **Container:** 1 Application = 1 App + Minimal Binaries (MBs of size).
Containers share the host's OS Kernel, meaning you can fit dozens of containers on a server that could only hold two or three VMs.

#### 3. Speed (Instant Startup)

A VM has to boot up an entire Operating System (BIOS, device drivers, init processes), which takes minutes. A container is just a process starting up; it creates a sandboxed environment in milliseconds. This allows apps to scale up instantly when traffic spikes.

#### 4. Isolation

If one container crashes or is hacked, it is isolated from the others. You can run `Python 2.7` in one container and `Python 3.0` in another on the very same machine without any conflict.

#### 5. Immutable Infrastructure

In the old days, if a server had a bug, an admin would SSH in and try to "patch" it live. This often caused "configuration drift" where servers became unique "snowflakes."
With containers, you never patch a running container. You simply fix the code, build a *new* image, kill the old container, and start the new one. This ensures the system is always in a known, clean state.

## Docker Images
A Docker image consists of a series of read-only layers.

-   **Base Image:** The foundational layer (e.g., Alpine Linux, Ubuntu). It provides the core OS filesystem and is optimized for size.
-   **Intermediate Layers:** Contain dependencies, libraries, and configuration instructions added on top of the base image.
-   **Application Layer:** The final layer containing your application code and entrypoint.

## Container vs. Image

Understanding the distinction between a **Container Image** and a **Container** is fundamental.

### 1. What is a Container Image?
An **Image** is a static, read-only template that includes everything needed to run an application: code, runtime, libraries, environment variables, and configuration files.

*   **State:** Inert (exists on disk). It does not consume CPU or memory until run.
*   **Mutability:** Immutable. You cannot change an image once it is built; you must build a new one to apply changes.
*   **Architecture:** Built from stacked read-only layers (OS, Dependencies, Code).

### 2. What is a Container?
A **Container** is a running instance of an image. It brings the static image to life as an isolated process.

*   **State:** Active (consumes CPU & Memory). You can start, stop, and interact with it.
*   **Mutability:** Ephemeral. It adds a thin Read/Write layer on top of the image. Changes made here are lost when the container is removed, unless persisted in Volumes.
*   **Layering:** The container layer sits on top of the image layers, allowing modification without altering the underlying image.

## Docker vs Virtual Machine

### The Anatomy of an Operating System

An operating system is built in layers, each abstracting complexity from the layer above it. At the foundation sits the kernel, which directly manages hardware resources like CPU, memory, storage, and network devices. The kernel handles critical tasks: scheduling processes, allocating memory, managing file systems, and controlling device drivers.

Above the kernel are system libraries that provide standardized interfaces for applications to interact with kernel services without needing to know hardware details. Then come system utilities and services—background processes that handle logging, networking, user authentication, and other housekeeping tasks. At the top layer are user applications that people actually interact with.

This layered architecture exists because hardware is complex and diverse. The OS creates a consistent, stable interface so developers don't need to write different code for every processor or storage device.

```mermaid
graph LR
    subgraph "Operating System Layers"
        A[User Applications<br/>Web Browsers, IDEs, Games]
        B[System Utilities & Services<br/>Logging, Networking, Authentication]
        C[System Libraries<br/>libc, libssl, Standard APIs]
        D[Kernel<br/>Process Scheduling, Memory Management,<br/>File Systems, Device Drivers]
        E[Hardware<br/>CPU, RAM, Storage, Network Devices]
    end
    
    A --> B
    B --> C
    C --> D
    D --> E
    
    style A fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    style B fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style C fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style D fill:#ffebee,stroke:#c62828,stroke-width:2px
    style E fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
```

### What virtualization targets?
Virtualization emerged to solve a fundamental problem: how to run multiple isolated operating systems on a single physical machine. Traditional virtualization (using hypervisors) virtualizes the hardware layer itself.

A hypervisor like VMware ESXi or KVM sits between the physical hardware and guest operating systems, presenting each guest with what appears to be complete, dedicated hardware—virtual CPUs, virtual memory, virtual network cards, virtual disks. Each virtual machine (VM) runs its own full operating system kernel, completely unaware it's sharing physical resources with other VMs. The hypervisor manages this illusion, translating virtual hardware operations into physical ones and ensuring isolation between guests.

This is powerful but heavyweight. Each VM needs its own complete OS installation, which means gigabytes of disk space, significant memory overhead, and minutes to boot up. You're essentially running multiple complete computers on one physical machine.

```mermaid
graph LR
    subgraph VM1["Virtual Machine 1"]
        A1[App A]
        B1[Guest OS<br/>Full Linux Kernel]
    end
    
    subgraph VM2["Virtual Machine 2"]
        A2[App B]
        B2[Guest OS<br/>Full Windows Kernel]
    end
    
    subgraph VM3["Virtual Machine 3"]
        A3[App C]
        B3[Guest OS<br/>Full Linux Kernel]
    end
    
    H[Hypervisor<br/>VMware ESXi / KVM<br/>Hardware Virtualization Layer]
    HW[Physical Hardware<br/>CPU, RAM, Storage, Network]
    
    A1 --> B1
    A2 --> B2
    A3 --> B3
    
    B1 --> H
    B2 --> H
    B3 --> H
    H --> HW
    
    style A1 fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    style A2 fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    style A3 fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    style B1 fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style B2 fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style B3 fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style H fill:#ffebee,stroke:#c62828,stroke-width:2px
    style HW fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
```

### Containers: Virtualizing at a Different Layer
Containers take a fundamentally different approach. Instead of virtualizing hardware, they virtualize at the operating system layer. All containers on a host share the same kernel but are isolated from each other through kernel features like namespaces and cgroups.

When you run a container, you're not booting a new operating system. You're creating an isolated process environment that has its own view of the filesystem, network interfaces, process tree, and user IDs—but it's all managed by the host's single kernel. The container engine (like Docker) uses Linux namespaces to create these isolated views and cgroups to limit resource usage.

This is why containers are so lightweight. A container image only needs the application code, its dependencies, and minimal filesystem utilities. It doesn't need kernel, drivers, or init systems. Containers typically measure in megabytes rather than gigabytes, start in seconds rather than minutes, and use far less memory.

```mermaid
graph LR
    subgraph C1["Container 1"]
        A1[App A]
        L1[Libraries &<br/>Dependencies]
    end
    
    subgraph C2["Container 2"]
        A2[App B]
        L2[Libraries &<br/>Dependencies]
    end
    
    subgraph C3["Container 3"]
        A3[App C]
        L3[Libraries &<br/>Dependencies]
    end
    
    CE[Container Engine<br/>Docker / containerd<br/>Namespaces & cgroups]
    HOS[Host OS Kernel<br/>Shared Linux Kernel]
    HW[Physical Hardware<br/>CPU, RAM, Storage, Network]
    
    A1 --> L1
    A2 --> L2
    A3 --> L3
    
    L1 --> CE
    L2 --> CE
    L3 --> CE
    CE --> HOS
    HOS --> HW
    
    style A1 fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    style A2 fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    style A3 fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    style L1 fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style L2 fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style L3 fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style CE fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style HOS fill:#ffebee,stroke:#c62828,stroke-width:2px
    style HW fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
```

## Docker Architecture: Components and How They Work Together

At its core, Docker uses a **client-server architecture**. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers.

These two components can run on the same system, or you can connect a Docker client to a remote Docker daemon. They communicate using a REST API, over UNIX sockets or a network interface.

Here is a breakdown of the critical components and how they interact.

---

### 1. The Core Components

#### A. The Docker Client (`docker`)

This is the primary way many Docker users interact with Docker. When you use commands such as `docker run`, the client sends these commands to `dockerd` (the daemon), which carries them out. The `docker` command uses the Docker API. The Docker client can communicate with more than one daemon.

#### B. The Docker Host (The Daemon)

The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

> **Note on Runtimes:** Under the hood, the daemon relies on lower-level tools to actually interface with the OS kernel.
> * **containerd:** A high-level runtime that manages the container lifecycle (pulling images, storage, execution).
> * **runc:** A low-level CLI tool that actually spawns and runs containers according to the OCI (Open Container Initiative) specification.
> 
> 

#### C. Docker Registries

A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can also run your own private registry.

* **Public:** Docker Hub.
* **Private:** AWS ECR, Google Artifact Registry, Azure ACR, or self-hosted.

---

### 2. Docker Objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects.

| Object | Description | Analogy |
| --- | --- | --- |
| **Image** | A read-only template with instructions for creating a Docker container. Often based on another image, with some additional customization. | The "Recipe" or "Class" definition. |
| **Container** | A runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. | The "Cake" or "Object" instance. |
| **Volume** | The preferred mechanism for persisting data generated by and used by Docker containers. | An external hard drive plugged into the container. |
| **Network** | Allows containers to communicate with each other and the outside world. | Virtual cabling connecting machines. |

---

### 3. How They Work Together: The Lifecycle

To understand the architecture in motion, let's trace what happens when you run a simple command like:
`docker run -i -t ubuntu /bin/bash`

#### Step 1: The Request

The **Docker Client** parses your command. It translates `run` into an API request and sends it to the **Docker Daemon** (`dockerd`).

#### Step 2: Image Check & Pull

The Daemon checks its local image cache.

* **If the `ubuntu` image exists locally:** It proceeds to the next step.
* **If the image is missing:** The Daemon talks to the **Docker Registry** (e.g., Docker Hub), downloads (pulls) the `ubuntu` image, and stores it locally.

#### Step 3: Container Creation

The Daemon creates a new **Container** from that image. It essentially creates a writeable layer on top of the read-only image layers.

#### Step 4: Runtime Execution

The Daemon instructs **containerd** to start the container. `containerd` uses **runc** to interface with the Linux kernel (using Namespaces for isolation and Control Groups for resource limiting) to create the isolated process.

#### Step 5: I/O Stream

Because you used the `-i` (interactive) and `-t` (pseudo-TTY) flags, the Daemon connects the input/output streams of the container back to your **Client** terminal. You now see the bash prompt inside the container.

---

### 4. Key Underlying Technologies

Docker is written in the Go programming language and takes advantage of several features of the Linux kernel to deliver its functionality:

* **Namespaces:** The technology that provides the *isolation*. Docker uses namespaces to provide the isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container (e.g., `pid` for processes, `net` for networking).
* **Control Groups (cgroups):** The technology that provides *resource management*. This allows the Docker engine to share available hardware resources to containers and optionally enforce limits and constraints (e.g., limiting a container to 512MB of RAM).
* **Union File Systems (UnionFS):** Efficient file systems that operate by creating layers, making them very lightweight and fast. This is why Docker images are built in layers.

---

## Essential Docker Commands

### Image Management

#### Pulling Images from a Registry
To download an image from Docker Hub (or another configured registry):

```bash
docker pull redis
```

This command downloads the `redis` image with the `latest` tag to your local machine.

#### Listing Local Images
To view all images stored locally:

```bash
docker images
```

This displays a table showing:
- Repository name
- Tag
- Image ID
- Creation date
- Size

#### Pulling Specific Image Versions
To pull a specific version (tag) of an image:

```bash
docker run redis:8.4.0
```

If the image doesn't exist locally, `docker run` automatically pulls it before creating the container.

---

### Container Lifecycle Management

#### Creating and Running Containers

**Foreground Mode (Attached):**
```bash
docker run redis
```

This starts a Redis container in the foreground. The terminal is attached to the container's output stream. Pressing `CTRL+C` sends a stop signal to the container.

**Detached Mode (Background):**
```bash
docker run -d redis
```

The `-d` flag runs the container in detached mode, returning control to your terminal immediately. The container runs in the background.

#### Viewing Running Containers
To list all currently running containers:

```bash
docker ps
```

Output includes:
- Container ID
- Image name
- Command
- Creation time
- Status
- Ports
- Container name

**Viewing All Containers (Including Stopped):**
```bash
docker ps -a
```

The `-a` flag shows all containers regardless of their state (running, stopped, exited).

#### Stopping Containers
To gracefully stop a running container:

```bash
docker stop <container_id>
```

Replace `<container_id>` with the actual container ID (visible from `docker ps`). Docker sends a `SIGTERM` signal, allowing the process to shut down gracefully. If the container doesn't stop within the timeout period (default 10 seconds), Docker sends a `SIGKILL` signal.

**Example:**
```bash
docker stop 3f2a1b8c9d4e
```
#### Starting Stopped Containers
To restart a stopped container:

```bash
docker start <container_id>
```
#### Removing Containers
To delete a stopped container:

```bash
docker rm <container_id>
```

To force-remove a running container:

```bash
docker rm -f <container_id>
```

---

## Port Mapping: Connecting Containers to the Outside World

### Understanding Container Networking Isolation

By default, Docker containers run in an **isolated network environment**. This means:

*   Each container has its own network namespace with its own network interfaces, IP addresses, and routing tables.
*   A container can expose services on any port internally (e.g., a web server listening on port 80).
*   However, these ports are **not accessible from the host machine or external networks** unless explicitly mapped.

This isolation is a security feature—containers cannot accidentally expose services to the outside world.
### The Concept: Host Port vs. Container Port

To make a containerized service accessible, you must create a **port mapping** (also called port binding or port publishing) that bridges the container's isolated network to the host's network.

**Container Port:** The port on which the application inside the container is listening.
*   Example: An Nginx web server inside a container listens on port `80` by default.

**Host Port:** The port on the host machine that forwards traffic to the container port.
*   Example: You might map host port `8080` to container port `80`, so accessing `localhost:8080` on your machine routes traffic to the container's port `80`.

**The Mapping:** `<host_port>:<container_port>`

When you map ports, Docker sets up **iptables rules** (on Linux) or equivalent networking rules to forward traffic from the host port to the container port.

---
### Port Mapping Syntax

The `-p` (or `--publish`) flag is used to map ports when running a container:

```bash
docker run -p <host_port>:<container_port> <image>
```

**Example:**
```bash
docker run -p 8080:80 nginx
```

This command:
1.  Starts an Nginx container.
2.  Nginx inside the container listens on port `80`.
3.  Maps host port `8080` to container port `80`.
4.  You can now access Nginx by visiting `http://localhost:8080` in your browser.

---

### Common Port Mapping Patterns

#### 1. Single Port Mapping
Map one host port to one container port:

```bash
docker run -d -p 3000:3000 node-app
```

*   Host port `3000` → Container port `3000`
*   Access via: `http://localhost:3000`

#### 2. Multiple Port Mappings
Map multiple ports by using multiple `-p` flags:

```bash
docker run -d -p 8080:80 -p 8443:443 nginx
```

*   Host port `8080` → Container port `80` (HTTP)
*   Host port `8443` → Container port `443` (HTTPS)

#### 3. Mapping to Different Host Ports
Useful when running multiple instances of the same service:

```bash
docker run -d -p 9001:80 --name web1 nginx
docker run -d -p 9002:80 --name web2 nginx
```

*   `web1` accessible at `http://localhost:9001`
*   `web2` accessible at `http://localhost:9002`

Both containers run Nginx on port `80` internally, but they're accessible on different host ports.

#### 4. Binding to Specific Host Interfaces
By default, Docker binds to all network interfaces (`0.0.0.0`). You can bind to a specific IP:

```bash
docker run -d -p 127.0.0.1:8080:80 nginx
```

*   Only accessible from `localhost` (not from external networks).
*   Useful for security when you don't want external access.

To bind to a specific network interface:
```bash
docker run -d -p 192.168.1.100:8080:80 nginx
```

#### 5. Random Host Port Assignment
Let Docker automatically assign an available host port:

```bash
docker run -d -P nginx
```

The `-P` (capital P) flag publishes **all exposed ports** to random available host ports.

To see which ports were assigned:
```bash
docker ps
```

Or:
```bash
docker port <container_id>
```

**Example output:**
```
80/tcp -> 0.0.0.0:32768
443/tcp -> 0.0.0.0:32769
```

---

### Inspecting Port Mappings

#### View Port Mappings for Running Containers
```bash
docker ps
```

The `PORTS` column shows the mappings:
```
CONTAINER ID   IMAGE     PORTS                  
abc123def456   nginx     0.0.0.0:8080->80/tcp
```

This means host port `8080` is mapped to container port `80`.

#### View Detailed Port Information
```bash
docker port <container_name_or_id>
```

**Example:**
```bash
docker port web1
```

**Output:**
```
80/tcp -> 0.0.0.0:8080
```

#### Inspect Full Container Configuration
```bash
docker inspect <container_id>
```

Look for the `"Ports"` section in the JSON output for detailed networking information.

---

### Practical Examples

#### Example 1: Running a Web Server
Run an Nginx web server accessible on port `8080`:

```bash
docker run -d -p 8080:80 --name my-nginx nginx
```

*   Visit `http://localhost:8080` in your browser.
*   The Nginx default page should appear.

#### Example 2: Running a Database
Run a PostgreSQL database accessible on the default port `5432`:

```bash
docker run -d -p 5432:5432 --name my-postgres -e POSTGRES_PASSWORD=secret postgres
```

*   Connect to the database using: `localhost:5432`
*   Username: `postgres`, Password: `secret`

#### Example 3: Running Multiple Redis Instances
Run two Redis instances on different ports:

```bash
docker run -d -p 6379:6379 --name redis1 redis
docker run -d -p 6380:6379 --name redis2 redis
```

*   `redis1` accessible on `localhost:6379`
*   `redis2` accessible on `localhost:6380`

#### Example 4: Development Environment
Run a Node.js app with live reload on port `3000`:

```bash
docker run -d -p 3000:3000 -v $(pwd):/app --name node-dev node:18
```

*   Maps port `3000`
*   Mounts current directory for live code updates

---

### Understanding the Network Flow

When you access `http://localhost:8080`:

1.  **Request hits the host** on port `8080`.
2.  **Docker's iptables rules** intercept the traffic.
3.  **Traffic is forwarded** to the container's network namespace.
4.  **The container receives** the request on port `80`.
5.  **The application** (e.g., Nginx) processes the request.
6.  **Response travels back** through the same path.

This happens transparently—the application inside the container doesn't know it's being accessed via port `8080`.

---

### Common Pitfalls and Solutions

#### Problem 1: Port Already in Use
**Error:**
```
Error response from daemon: driver failed programming external connectivity: 
Bind for 0.0.0.0:8080 failed: port is already allocated
```

**Solution:**
*   Another process (or container) is using port `8080`.
*   Check what's using the port: `lsof -i :8080` (macOS/Linux) or `netstat -ano | findstr :8080` (Windows)
*   Use a different host port: `docker run -p 8081:80 nginx`

#### Problem 2: Cannot Access Container from External Network
**Issue:** You can access the container from `localhost` but not from another machine.

**Solution:**
*   Ensure you're binding to `0.0.0.0` (all interfaces), not `127.0.0.1`:
    ```bash
    docker run -p 0.0.0.0:8080:80 nginx
    ```
*   Check firewall rules on the host machine.

#### Problem 3: Wrong Port Mapping Order
**Mistake:**
```bash
docker run -p 80:8080 nginx  # WRONG
```

This tries to map host port `80` to container port `8080`, but Nginx listens on `80` inside the container.

**Correct:**
```bash
docker run -p 8080:80 nginx  # CORRECT
```

---

### Port Mapping with Docker Compose

In a `docker-compose.yml` file, port mappings are defined under the `ports` section:

```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8080:80"      # host:container
      - "8443:443"
  
  db:
    image: postgres
    ports:
      - "5432:5432"
    environment:
      POSTGRES_PASSWORD: secret
```

Run with:
```bash
docker-compose up -d
```

---

### Summary

| Concept | Syntax | Example |
| :--- | :--- | :--- |
| Basic port mapping | `-p <host>:<container>` | `-p 8080:80` |
| Multiple ports | Multiple `-p` flags | `-p 8080:80 -p 8443:443` |
| Bind to specific IP | `-p <ip>:<host>:<container>` | `-p 127.0.0.1:8080:80` |
| Random port assignment | `-P` | `-P` (publishes all exposed ports) |
| View port mappings | `docker ps` or `docker port <id>` | `docker port my-nginx` |

Port mapping is fundamental to making containerized applications accessible and is one of the most commonly used Docker features in both development and production environments.

---

## Docker Debugging and Troubleshooting

When working with containers, debugging is essential for diagnosing issues, monitoring performance, and understanding container behavior. Docker provides several powerful commands for troubleshooting.

### 1. Viewing Container Logs

| Command | Purpose | Example |
| :--- | :--- | :--- |
| `docker logs <container>` | View all container logs | `docker logs my-nginx` |
| `docker logs -f <container>` | Follow logs in real-time (like `tail -f`) | `docker logs -f my-nginx` |
| `docker logs --tail N <container>` | View last N lines | `docker logs --tail 50 my-nginx` |
| `docker logs -t <container>` | Show logs with timestamps | `docker logs -t my-nginx` |
| `docker logs --since TIME <container>` | Show logs since specific time | `docker logs --since 30m my-nginx` |
| `docker logs -f --tail 100 --since 10m <container>` | Combine options | `docker logs -f --tail 100 --since 10m my-nginx` |

---

### 2. Executing Commands Inside Running Containers

| Command | Purpose | Example |
| :--- | :--- | :--- |
| `docker exec -it <container> /bin/bash` | Interactive shell access (bash) | `docker exec -it my-nginx /bin/bash` |
| `docker exec -it <container> /bin/sh` | Interactive shell (for Alpine images) | `docker exec -it my-nginx /bin/sh` |
| `docker exec <container> <command>` | Run single command | `docker exec my-nginx ls -la /usr/share/nginx/html` |
| `docker exec <container> env` | Check environment variables | `docker exec my-nginx env` |
| `docker exec <container> ps aux` | Check running processes | `docker exec my-nginx ps aux` |
| `docker exec <container> ping -c 3 <host>` | Test network connectivity | `docker exec my-nginx ping -c 3 google.com` |
| `docker exec -u <user> <container> <command>` | Run command as specific user | `docker exec -u root my-nginx whoami` |

---

## `docker run` vs `docker start`

### Key Difference

**`docker run`** creates a **new container** from an image and starts it.  
**`docker start`** starts an **existing stopped container**.

### Comparison Table

| Aspect | `docker run` | `docker start` |
| :--- | :--- | :--- |
| **Purpose** | Create + Start a new container | Start an existing stopped container |
| **Input** | Image name (e.g., `nginx`) | Container ID or name |
| **Creates new container?** | Yes | No |
| **Use case** | First time running an image | Restarting a stopped container |
| **Example** | `docker run -d nginx` | `docker start my-nginx` |

### Workflow Example

```bash
# Step 1: Create and start a new container from nginx image
docker run -d --name web nginx

# Step 2: Stop the container
docker stop web

# Step 3: Start the existing container again (NOT docker run)
docker start web
```

**Key Takeaway:** Use `docker run` once to create a container, then use `docker start`/`docker stop` to control its lifecycle.

---

## Docker Demo Project Overview

Consider a practical scenario where a developer is building a Node.js application with the following workflow:

1. **Local Development:** The application requires MongoDB as its database. Instead of installing MongoDB directly on the host machine, the developer runs it as a Docker container, ensuring environment isolation and consistency.

2. **Source Control:** The developer commits the application code to a Git repository (e.g., GitHub, GitLab).

3. **CI/CD Pipeline:** A code push triggers a Jenkins pipeline that performs the following:
   - Builds a Docker image from the application code
   - Runs automated tests to verify functionality
   - Upon successful validation, pushes the image to a private Docker registry (Amazon ECR)

4. **Deployment:** The development server pulls the validated application image from the private registry and the MongoDB image from Docker Hub. Both containers are deployed and configured to communicate with each other via Docker networking.

---

## Docker Networks in the Demo Project

### What is a Docker Network?

A Docker network allows containers to communicate with each other in an isolated virtual network. By default, containers cannot reach each other unless they are on the same network.

### Network Setup Example

#### 1. Create Network
```bash
docker network create mongo-network
```

#### 2. Start MongoDB Container
```bash
docker run -d \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  --net mongo-network \
  --name mongodb \
  mongo
```

**Configuration:**
- **Port:** `27017` (MongoDB default)
- **Credentials:** `admin:password`
- **Network:** `mongo-network`

#### 3. Start Mongo Express (Web UI)
```bash
docker run -d \
  -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
  -e ME_CONFIG_BASICAUTH_USERNAME=user \
  -e ME_CONFIG_BASICAUTH_PASSWORD=pass \
  -e ME_CONFIG_MONGODB_SERVER=mongodb \
  --net mongo-network \
  --name mongo-express \
  mongo-express
```

**Configuration:**
- **Web UI:** `http://localhost:8081`
- **Login:** `user:pass`
- **MongoDB Connection:** Uses container name `mongodb` (DNS resolution)

**Key Insight:** Both containers communicate via `mongo-network`. Mongo Express connects to MongoDB using the container name `mongodb` instead of IP addresses.

### Application Setup: Database and API Configuration

#### Database Initialization

Using the Mongo Express web interface at `http://localhost:8081`, create the following database structure:

1. **Database Name:** `user-account`
2. **Collection Name:** `users`

This collection will store user profile data for the application.

#### Application Connection String

The Node.js application connects to MongoDB using the following connection string:

```
mongodb://admin:password@localhost:27017
```

**Connection String Breakdown:**
- **Protocol:** `mongodb://`
- **Credentials:** `admin:password` (username:password)
- **Host:** `localhost` (since we're running locally and have port-mapped 27017)
- **Port:** `27017` (MongoDB default port)

> **Note:** This connection approach is simplified for demonstration purposes. In production environments, use environment variables for credentials, implement connection pooling, and follow security best practices such as using MongoDB Atlas or properly secured self-hosted instances.

#### API Endpoints

The application exposes two RESTful API endpoints for user profile management:

| Endpoint | Method | Purpose | Description |
| :--- | :--- | :--- | :--- |
| `/get-profile` | GET | Retrieve user profile | Fetches user data from the `users` collection |
| `/update-profile` | POST | Update user profile | Updates or creates user profile data (upsert operation) |

**Reference Implementation:** [View source code](https://github.com/pritommazumder/devops-a-complete-guide/tree/main/codes/docker/js-app)

## Docker Compose files
As we can see, we have to write long and complicated docker run commands to start our containers. This is not a good practice.

However we can replace these complicated docker run commands with docker compose files. It has many benefits.
- It helps to structure commands.
- Containers, networks, volumes and configuration all can be done using a YAML file
- Declarative appraoch: We declare what we want to do, not how to do it.
- Communication via container name
- By default, Compose sets up a single network for our app

### Docker CLI to Docker Compose: Conversion Rules

Converting Docker CLI commands to Docker Compose follows a systematic mapping of flags and options to YAML properties. Below is a comprehensive rule-based conversion guide.

#### Rule-Based Mapping Table

| Docker CLI Flag | Docker Compose Property | YAML Location | Example CLI | Example Compose |
| :--- | :--- | :--- | :--- | :--- |
| `image_name` (positional) | `image:` | `services.<name>.image` | `mongo` | `image: mongo` |
| `--name container_name` | Service name | `services.<name>` | `--name mongodb` | `mongodb:` (service key) |
| `-p host:container` | `ports:` | `services.<name>.ports` | `-p 27017:27017` | `- "27017:27017"` |
| `-e KEY=VALUE` | `environment:` | `services.<name>.environment` | `-e MONGO_INITDB_ROOT_USERNAME=admin` | `- MONGO_INITDB_ROOT_USERNAME=admin` |
| `--net network_name` | `networks:` | `services.<name>.networks` | `--net mongo-network` | `networks: [mongo-network]` |
| `-d` (detached) | N/A | Implicit | `-d` | *(Always runs detached)* |
| `-v host:container` | `volumes:` | `services.<name>.volumes` | `-v mongo-data:/data/db` | `- mongo-data:/data/db` |
| `--restart policy` | `restart:` | `services.<name>.restart` | `--restart always` | `restart: always` |
| N/A | `depends_on:` | `services.<name>.depends_on` | N/A | `depends_on: [mongodb]` |

---

#### Conversion Process: Step-by-Step

**Step 1: Identify the Image**
```bash
docker run mongo
```
↓
```yaml
services:
  service-name:
    image: mongo
```

**Step 2: Map Container Name to Service Name**
```bash
docker run --name mongodb mongo
```
↓
```yaml
services:
  mongodb:  # Container name becomes service name
    image: mongo
```

**Step 3: Convert Port Mappings**
```bash
docker run -p 27017:27017 mongo
```
↓
```yaml
services:
  mongodb:
    image: mongo
    ports:
      - "27017:27017"  # Always use quotes for port mappings
```

**Step 4: Convert Environment Variables**
```bash
docker run -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password mongo
```
↓
```yaml
services:
  mongodb:
    image: mongo
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
    # Alternative syntax:
    # environment:
    #   MONGO_INITDB_ROOT_USERNAME: admin
    #   MONGO_INITDB_ROOT_PASSWORD: password
```

**Step 5: Convert Network Configuration**
```bash
docker network create mongo-network
docker run --net mongo-network mongo
```
↓
```yaml
services:
  mongodb:
    image: mongo
    networks:
      - mongo-network

networks:
  mongo-network:
    driver: bridge  # Default network driver
```

**Step 6: Convert Volume Mounts**
```bash
docker run -v mongo-data:/data/db mongo
```
↓
```yaml
services:
  mongodb:
    image: mongo
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
    driver: local  # Default volume driver
```

---

### Complete Conversion Example: MongoDB + Mongo Express
```yaml
version: '3'
services:
  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
  mongo-express:
    image: mongo-express
    ports:
      - 8081:8081
    restart: always
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
```

---

### Conversion Comparison Table

| Aspect | Docker CLI | Docker Compose |
| :--- | :--- | :--- |
| **Command Count** | 3 commands (network create + 2 × docker run) | 1 file |
| **Network Creation** | Manual (`docker network create`) | Automatic (declared in YAML) |
| **Container Naming** | `--name` flag | Service name (key in `services:`) |
| **Startup Order** | Manual (run MongoDB first) | Automatic (`depends_on:`) |
| **Configuration Management** | Flags in terminal | Declarative YAML file |
| **Reproducibility** | Copy/paste commands | Version-controlled file |
| **Multi-container Coordination** | Manual orchestration | Single `docker-compose up` |
| **Network DNS** | Manual `--net` flag | Automatic (all services in same network) |
| **Volume Management** | Manual creation or inline | Declared in `volumes:` section |

---

### Running the Docker Compose Stack

```bash
# Start all services (detached mode)
docker-compose -f mongo.yaml up
```

### Stopping the Docker Container
```bash
docker-compose -f mongo.yaml down
```

---

### Data Persistence and Container Ephemerality

A fundamental characteristic of Docker containers is their **ephemeral nature**.

**The Challenge:**
Data written inside a container lives in a temporary writable layer. When a container is stopped and removed, that data is destroyed. Even if you start a new container from the exact same image, it instantiates with a fresh, clean filesystem.

**The Solution:**
To ensure data survives container restarts and deletions (crucial for databases like MongoDB), we use **Docker Volumes**. This mechanism decouples data from the container lifecycle.

> *We will explore Docker Volumes and storage strategies in depth in the upcoming section.*

---

## What is a Dockerfile? A Complete Overview

### Definition

A **Dockerfile** is a blueprint for building Docker images. It is a text file containing a set of instructions used to assemble a Docker image in an automated and reproducible manner.

---

### Core Dockerfile Instructions

| Instruction | Purpose | Syntax | Example | Execution Context | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **FROM** | Specifies the base image upon which the new image will be built. Must be the first instruction in a Dockerfile. | `FROM <image>:<tag>` | `FROM node:18-alpine` | Build time | For a JavaScript backend, using `node` as the base image ensures Node.js is pre-installed in the container environment. |
| **ENV** | Defines environment variables that will be available within the container. | `ENV <KEY>=<VALUE>` | `ENV MONGO_DB_USERNAME=admin \`<br>`    MONGO_DB_PWD=password` | Build time + Runtime | While environment variables can be set in Docker Compose files, defining them in the Dockerfile bakes them into the image itself. |
| **RUN** | Executes commands during the image build process. Used for installing packages, creating directories, or system configurations. | `RUN <command>` | `RUN mkdir -p /home/app` | Build time (inside container) | Commands operate within the container's filesystem, not on the host machine. Multiple `RUN` instructions can be used throughout a Dockerfile. |
| **COPY** | Copies files and directories from the host machine into the container's filesystem. | `COPY <source_path> <destination_path>` | `COPY . /home/app` | Build time (from host to container) | Operates on the **host machine**, transferring files into the image being built. Source path is relative to the build context. |
| **CMD** | Specifies the default command that will be executed when a container starts from the image. | `CMD ["executable", "param1", "param2"]` | `CMD ["node", "server.js"]` | Runtime (when container starts) | Only one `CMD` instruction is executed (last one takes precedence). This is the entry point that runs your application. |

---

### Key Differences: RUN vs CMD

| Aspect | `RUN` | `CMD` |
| :--- | :--- | :--- |
| **Purpose** | Executes commands during image build | Defines the default command to run when container starts |
| **Execution Time** | Build time | Runtime (when container starts) |
| **Frequency** | Can appear multiple times in a Dockerfile | Only one `CMD` instruction is executed (last one takes precedence) |
| **Use Case** | Installing dependencies, setting up environment | Starting the application process |
| **Example** | `RUN npm install` — Installs dependencies during build | `CMD ["node", "app.js"]` — Starts the Node.js application when container runs |

### Image Layers

Docker images are composed of multiple layers stacked hierarchically. Each layer represents a filesystem change or instruction from the Dockerfile. Understanding this layered architecture is crucial for optimizing image size and build performance.

**Layer Hierarchy Example:**

The application image (`app:1.0`) demonstrates the following layer dependency chain:

- **Application Layer:** `app:1.0` (custom application code)
  - **Base Runtime Layer:** `node:25-alpine` (Node.js runtime environment)
    - **Operating System Layer:** `alpine:3.23` (minimal Linux distribution optimized for containers)


### Understanding WORKDIR and COPY Path Resolution

**Initial Dockerfile (Problematic)**

```dockerfile
FROM node:25-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

RUN mkdir -p /home/app
COPY . /home/app
CMD ["node", "server.js"]
```

Running `docker run my-app:1.0` produces: `Error: Cannot find module '/server.js'`

**Root Cause:** Without `WORKDIR`, `CMD` executes from `/` (root), not `/home/app`.

| Instruction | Execution Context |
|-------------|-------------------|
| `COPY . /home/app` | Files copied to `/home/app` |
| `CMD ["node", "server.js"]` | Node searches in `/` |

**Solution: Use WORKDIR**

`WORKDIR` sets the working directory for `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions.

```dockerfile
FROM node:25-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

WORKDIR /home/app
COPY . .
CMD ["node", "server.js"]
```

**Second Issue: Directory Structure Mismatch**

If the host structure is:
```
project/
├── Dockerfile
├── app/
│   └── server.js
└── docker-compose.yaml
```

Then `COPY . .` results in:
```
/home/app/
├── app/server.js    ← Nested!
└── ...
```

`CMD` looks for `/home/app/server.js`, but file exists at `/home/app/app/server.js`.

**Final Dockerfile**

```dockerfile
FROM node:25-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

WORKDIR /home/app
COPY ./app .
CMD ["node", "server.js"]
```

`COPY ./app .` copies the *contents* of `app/` directly into `/home/app`, placing `server.js` at the expected path.

### Optimizing Image Size with .dockerignore

When building Docker images, the `COPY` instruction transfers all files from the source directory into the image. This includes development artifacts like `node_modules`, which can significantly bloat image size and increase build times.

**The Problem:**
- `node_modules` directories are often large (hundreds of MBs)
- Dependencies should be installed fresh during the build to ensure consistency across environments
- Copying local `node_modules` may include platform-specific binaries that won't work in the container

**The Solution: `.dockerignore`**

Similar to `.gitignore`, a `.dockerignore` file excludes files and directories from the build context:

```dockerignore
node_modules
app/node_modules
```

This prevents `node_modules` from being copied, reducing the build context size and ensuring clean dependency installation.

### Installing Dependencies at Build Time

With `node_modules` excluded, dependencies must be installed during the image build using the `RUN` instruction:

```dockerfile
FROM node:25-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

WORKDIR /home/app

COPY ./app .

RUN npm install

CMD ["node", "server.js"]
```

**Execution Flow:**

| Instruction | Phase | Purpose |
|-------------|-------|---------|
| `COPY ./app .` | Build time | Copies source code (excluding `node_modules`) |
| `RUN npm install` | Build time | Installs dependencies inside the image |
| `CMD ["node", "server.js"]` | Runtime | Starts the application when container runs |

**Benefits of this approach:**
- **Smaller build context:** Faster image builds due to reduced file transfer
- **Consistent dependencies:** Fresh installation ensures compatibility with the container's OS and architecture
- **Immutable images:** Dependencies are baked into the image, eliminating "works on my machine" issues

## Pushing Docker Image to AWS ECR

#### Prerequisites

1. **Install AWS CLI** (if not installed)

    ```bash
    brew install awscli
    ```

2. **Configure AWS CLI**

    ```bash
    aws configure
    ```

    You'll be prompted for:
    - AWS Access Key ID
    - AWS Secret Access Key
    - Default region name (e.g., `eu-central-1`)
    - Default output format (e.g., `json`)

3. **Add ECR Permissions to IAM User**

    - Go to AWS Console → IAM → Users → Your User → Add Permissions
    - Select "Attach policies directly"
    - Search for `AmazonEC2ContainerRegistryFullAccess`
    - Select it and click "Add permissions"

#### Steps to Push Image to ECR

Step 1: Create an ECR Repository (via AWS Console or CLI)

    aws ecr create-repository --repository-name js-app --region eu-central-1

Step 2: Authenticate Docker with ECR

    aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.eu-central-1.amazonaws.com

Step 3: Tag the local image for ECR

    docker tag js-app:latest <account-id>.dkr.ecr.eu-central-1.amazonaws.com/js-app:latest

Step 4: Push the image to ECR

    docker push <account-id>.dkr.ecr.eu-central-1.amazonaws.com/js-app:latest

Step 5: Verify the push (optional)

    aws ecr describe-images --repository-name js-app --region eu-central-1

#### Changing Image Tags

To use a different tag (e.g., version number instead of `latest`):

    docker tag js-app:latest <account-id>.dkr.ecr.eu-central-1.amazonaws.com/js-app:v1.0.0
    docker push <account-id>.dkr.ecr.eu-central-1.amazonaws.com/js-app:v1.0.0

_NOTE: Replace `<account-id>` with your 12-digit AWS Account ID_

## Pulling and Running Images from AWS ECR

After pushing an image to a private registry like AWS ECR, it needs to be pulled and run on a target environment (e.g., a development server). This section documents the complete workflow for simulating this process locally.

### The Deployment Scenario

In a typical CI/CD pipeline:
1. **CI/CD Pipeline** builds the Docker image and pushes it to AWS ECR
2. **Development/Production Server** pulls the image from ECR and runs it alongside other services (e.g., MongoDB)

This workflow can be simulated locally to test the deployment process before actual server deployment.

---

### Step 1: Authenticate Docker with ECR

Before pulling images from a private ECR repository, Docker must be authenticated:

```bash
aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin <IAM-id>.dkr.ecr.<aws-region-name>.amazonaws.com
```

**What this command does:**
- `aws ecr get-login-password` retrieves a temporary authentication token from AWS
- The token is piped to `docker login` to authenticate with the ECR registry
- Authentication is valid for 12 hours

---

### Step 2: Pull the Image from ECR

```bash
docker pull <IAM-id>.dkr.ecr.<aws-region-name>.amazonaws.com/<registry-name>:<tag>
```

This downloads the image from the private ECR repository to your local machine.

---

### Step 3: Run with Docker Compose

Since the application requires MongoDB, use Docker Compose to orchestrate multiple containers:

**docker-compose.yaml:**
```yaml
version: '3'
services:
  my-app:
    image: <IAM-id>.dkr.ecr.<aws-region-name>.amazonaws.com/<registry-name>:<tag>
    ports:
      - 3000:3000
  mongodb:
    image: mongo
    ports:
     - 27017:27017
    environment:
     - MONGO_INITDB_ROOT_USERNAME=admin
     - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
     - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    restart: always
    ports:
     - 8081:8081
    environment:
     - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
     - ME_CONFIG_MONGODB_ADMINPASSWORD=password
     - ME_CONFIG_MONGODB_SERVER=mongodb
    depends_on:
     - "mongodb"
volumes:
  mongo-data:
    driver: local
```

**Start all services:**
```bash
docker-compose up
```

Or run in detached mode (background):
```bash
docker-compose up -d
```

---

### Step 4: Verify the Deployment

Once the containers are running, verify the services:

| Service | URL | Purpose |
|---------|-----|---------|
| Node.js App | http://localhost:3000 | Main application |
| Mongo Express | http://localhost:8081 | MongoDB admin UI |
| MongoDB | localhost:27017 | Database (internal) |

**Check running containers:**
```bash
docker ps
```

**View logs:**
```bash
docker-compose logs -f my-app
```

---

### Understanding Container Networking

When using Docker Compose, all services are automatically placed on the same network and can communicate using their service names as hostnames.

**Connection Flow:**
```
my-app (Node.js) → mongodb:27017 → MongoDB Container
                   ↑
                   Uses service name, NOT localhost
```

**Why `localhost` doesn't work between containers:**

| Context | `localhost` refers to |
|---------|----------------------|
| Inside `my-app` container | The `my-app` container itself |
| Inside `mongodb` container | The `mongodb` container itself |
| On host machine | The host machine |

Containers must use **service names** (e.g., `mongodb`) defined in `docker-compose.yaml` to communicate with each other.

---

### Application Configuration for Docker Compose

The Node.js application must be configured to connect to MongoDB using the Docker Compose service name:

```javascript
// Use when running with Docker Compose
let mongoUrlDockerCompose = "mongodb://admin:password@mongodb:27017";

// Use when running locally (not in Docker)
let mongoUrlLocal = "mongodb://admin:password@localhost:27017";
```

The application at [codes/docker/js-app/app/server.js](../codes/docker/js-app/app/server.js) uses `mongoUrlDockerCompose` when deployed in containers.

---

## Docker Compose: Overview and Data Persistence

### What is Docker Compose?

Docker Compose is a tool for defining and running multi-container Docker applications. Instead of running multiple `docker run` commands with complex flags, you define your entire application stack in a single YAML file.

**Key Benefits:**
- **Declarative Configuration:** Define what you want, not how to do it
- **Single Command Deployment:** Start all services with `docker-compose up`
- **Automatic Networking:** Containers can communicate using service names
- **Easy Reproducibility:** Share the YAML file to replicate environments

### How Docker Compose Works

```
┌─────────────────────────────────────────────────────────────┐
│                    docker-compose.yaml                       │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │  Service 1  │  │  Service 2  │  │  Service 3  │          │
│  │  (my-app)   │  │  (mongodb)  │  │  (mongo-    │          │
│  │             │  │             │  │   express)  │          │
│  └─────────────┘  └─────────────┘  └─────────────┘          │
└─────────────────────────────────────────────────────────────┘
                           │
                           ▼
              docker-compose up -d
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  Docker Engine                               │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │ Container 1 │  │ Container 2 │  │ Container 3 │          │
│  │  (my-app)   │◄─┤  (mongodb)  │◄─┤  (mongo-    │          │
│  │   :3000     │  │   :27017    │  │   express)  │          │
│  └─────────────┘  └─────────────┘  │   :8081     │          │
│         │                │         └─────────────┘          │
│         └────────────────┴──────────────┘                   │
│                    Default Network                           │
│              (automatic DNS resolution)                      │
└─────────────────────────────────────────────────────────────┘
```

**Execution Flow:**
1. Docker Compose reads the YAML file
2. Creates a default network for all services
3. Pulls required images (if not available locally)
4. Creates and starts containers in dependency order
5. Services communicate via container names as DNS hostnames

### Data Persistence with Docker Volumes

This project uses Docker volumes to persist MongoDB data across container restarts.

**The Problem:** Container filesystems are ephemeral. When a container is removed, all data inside it is lost.

**The Solution:** Mount a Docker volume to store data outside the container's lifecycle.

#### Volume Configuration in docker-compose.yaml

```yaml
services:
  mongodb:
    image: mongo
    volumes:
      - mongo-data:/data/db    # Mount volume to MongoDB's data directory

volumes:
  mongo-data:                   # Declare the named volume
    driver: local
```

**How It Works:**

| Component | Value | Purpose |
|-----------|-------|---------|
| `mongo-data` | Named volume | Persistent storage managed by Docker |
| `/data/db` | Container path | Where MongoDB stores its database files |
| `driver: local` | Storage driver | Uses local disk storage |

#### Data Lifecycle

**Without Volume:**
```
Container created → Data written → Container removed → DATA LOST
```

**With Volume:**
```
Container created → Data written to volume → Container removed → DATA PRESERVED
                                                                      ↓
New container created → Mounts same volume → DATA AVAILABLE
```
#### Commands That Preserve Data

| Command | Effect on Data |
|---------|---------------|
| `docker-compose stop` | Data preserved (containers paused) |
| `docker-compose down` | Data preserved (containers removed, volumes kept) |
| `docker-compose restart` | Data preserved |
| `docker-compose down -v` | **DATA DELETED** (removes volumes) |

#### Useful Volume Commands

```bash
# List all volumes
docker volume ls

# Inspect volume details (see mount location)
docker volume inspect mongo-data

# Remove a specific volume (DELETES DATA)
docker volume rm mongo-data

# Remove all unused volumes
docker volume prune
```

---

## Docker Security, Size & Best Practices: In-Depth Guide

Building secure, efficient Docker images requires understanding why each practice matters. This section explains the reasoning behind each recommendation with practical examples.

---

### 1. Only Use Official/Trusted Docker Images as Base Images

**Why This Matters:**

Base images form the foundation of your container. A compromised base image means every container built from it inherits those vulnerabilities or malware. Attackers have been known to publish malicious images with names similar to popular official images (typosquatting).

**The Risk:**

```dockerfile
# ❌ DANGEROUS - Unknown publisher, could contain malware
FROM randomuser/node-server:latest

# ❌ RISKY - Typosquatting attack (note the typo)
FROM nod3:18-alpine
```

When you use an untrusted image, you're essentially running someone else's code with full access to your application and potentially your infrastructure.

**The Solution:**

```dockerfile
# ✅ SAFE - Official Node.js image maintained by Docker and Node.js team
FROM node:18-alpine

# ✅ SAFE - Official PostgreSQL image
FROM postgres:16-alpine

# ✅ SAFE - Verified publisher image (look for "Docker Official Image" badge)
FROM nginx:1.25-alpine
```

---

### 2. Use Specific Image Versions (Pin Your Versions)

**Why This Matters:**

The `latest` tag is mutable—it points to whatever the newest version is at any given time. This creates several problems:

1. **Unpredictable builds:** Your image today might differ from tomorrow's
2. **Breaking changes:** A new version might introduce incompatibilities
3. **Security auditing:** You can't track exactly what version is deployed
4. **Rollback difficulties:** Hard to reproduce a previous working state

**The Problem Illustrated:**

```dockerfile
# ❌ BAD - What version is "latest"? It changes constantly
FROM node:latest

# Timeline of disaster:
# Monday:    latest = 18.17.0 → Build works ✓
# Tuesday:   latest = 20.0.0  → Build breaks ✗ (breaking changes in Node 20)
# Wednesday: You can't figure out why production differs from staging
```

**The Solution:**

```dockerfile
# ✅ GOOD - Exact version, reproducible forever
FROM node:18.19.0-alpine3.19

# ✅ ACCEPTABLE - Minor version pinning (gets security patches)
FROM node:18.19-alpine

# ✅ ACCEPTABLE - Major version pinning (if you need flexibility)
FROM node:18-alpine
```

---
### 3. Use Minimal Base Images

**Why This Matters:**

Every package, library, and binary in your image is:
1. **A potential security vulnerability** - More code = more attack surface
2. **Additional size** - Slower pulls, more storage costs
3. **Unnecessary complexity** - Tools you'll never use

**Size Comparison:**

| Base Image | Size | Packages | Use Case |
|------------|------|----------|----------|
| `ubuntu:22.04` | ~77MB | ~100+ | Full OS, many tools |
| `debian:bookworm` | ~124MB | ~100+ | Full OS, stable |
| `debian:bookworm-slim` | ~74MB | ~50 | Reduced Debian |
| `alpine:3.19` | ~7MB | ~15 | Minimal, musl libc |
| `distroless/static` | ~2MB | None | Just your binary |
| `scratch` | 0MB | None | Empty, for static binaries |

**The Security Perspective:**

```dockerfile
# ❌ BAD - Full Ubuntu image
FROM ubuntu:22.04
# Contains: bash, apt, curl, wget, ssh, and 100+ other packages
# Each package is a potential CVE waiting to happen
# Attack surface: HUGE

# ✅ GOOD - Alpine-based image
FROM node:18-alpine
# Contains: Node.js, npm, minimal shell utilities
# Attack surface: SMALL
```

**Practical Example - Node.js Application:**

```dockerfile
# ❌ Full Node image - 1.1GB
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "server.js"]
# Result: 1.1GB+ image with Python, gcc, make, and tons of dev tools

# ✅ Alpine Node image - 180MB
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install --production
CMD ["node", "server.js"]
# Result: ~180MB image with just what you need
```

**For Compiled Languages (Go, Rust) - Use Distroless or Scratch:**

```dockerfile
# Build stage
FROM golang:1.21-alpine AS builder
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 go build -o server .

# ✅ BEST - Distroless image (no shell, no package manager)
FROM gcr.io/distroless/static-debian12
COPY --from=builder /app/server /
CMD ["/server"]
# Result: ~7MB image with ZERO attack surface beyond your app

# ✅ ULTIMATE - Scratch (empty) image
FROM scratch
COPY --from=builder /app/server /
CMD ["/server"]
# Result: ~5MB image (just your binary)
```

**When Alpine Might Not Work:**

Alpine uses `musl` libc instead of `glibc`. Some applications (especially those with native bindings) may have compatibility issues:

```dockerfile
# If Alpine causes issues, use slim variants instead
FROM node:18-slim       # Debian-based but minimal
FROM python:3.11-slim   # Smaller than full, uses glibc
```
---

### 4. Optimize Caching Image Layers

**Why This Matters:**

Docker builds images layer by layer. Each instruction creates a new layer. Docker caches these layers and reuses them if nothing has changed. Poor layer ordering means:
1. **Slower builds** - Rebuilding unchanged layers
2. **Larger images** - Duplicate data across layers
3. **Wasted bandwidth** - Re-pushing unchanged layers

**How Docker Layer Caching Works:**

```
Dockerfile Instructions:
    FROM node:18-alpine          → Layer 1 (cached if unchanged)
    WORKDIR /app                 → Layer 2 (cached if Layer 1 unchanged)
    COPY package.json .          → Layer 3 (invalidated if package.json changes)
    RUN npm install              → Layer 4 (invalidated if Layer 3 invalidated)
    COPY . .                     → Layer 5 (invalidated if ANY source file changes)
    CMD ["node", "server.js"]    → Layer 6 (invalidated if Layer 5 invalidated)
```

**Key Insight:** When a layer is invalidated, ALL subsequent layers must be rebuilt.

**The Problem:**

```dockerfile
# ❌ BAD - Copies everything first, then installs dependencies
FROM node:18-alpine
WORKDIR /app
COPY . .                    # Layer invalidated on ANY file change
RUN npm install             # Must reinstall ALL deps even for tiny code change
CMD ["node", "server.js"]

# Change one line of code → npm install runs again (slow!)
```

**The Solution:**

```dockerfile
# ✅ GOOD - Copy dependency files first, then source code
FROM node:18-alpine
WORKDIR /app

# Layer 1: Copy only dependency definitions
COPY package.json package-lock.json ./

# Layer 2: Install dependencies (cached unless package*.json changes)
RUN npm ci --only=production

# Layer 3: Copy source code (only this rebuilds on code changes)
COPY . .

CMD ["node", "server.js"]

# Change one line of code → only COPY . . runs again (fast!)
```

**Advanced Caching Strategies:**

```dockerfile
# Combine RUN commands to reduce layers
# ❌ BAD - Multiple layers
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y vim
RUN apt-get clean

# ✅ GOOD - Single layer, cleanup in same layer
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        vim && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
```

**Python Example:**

```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Copy requirements first (changes less frequently)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy source code (changes frequently)
COPY . .

CMD ["python", "app.py"]
```

---

### Complete Workflow Summary

```bash
# 1. Build image with updated code
docker build -t <IAM-id>.dkr.ecr.<aws-region>.amazonaws.com/<repository-name>:<tag>.

# 2. Authenticate with ECR
aws ecr get-login-password --region <aws-region> | docker login --username AWS --password-stdin <IAM-id>.dkr.ecr.<aws-region> .amazonaws.com

# 3. Push to ECR
docker push <IAM-id>.dkr.ecr.<aws-region> .amazonaws.com/<repository-name>:<tag>

# 4. Update docker-compose.yaml with new image tag

# 5. Pull and run (simulating deployment server)
docker-compose up
```

---