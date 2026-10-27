---
title: Containers
tags: [infrastructure, devops]
updated: 2026-10-27
---

# Containers

A container packages an application with everything it needs (libraries, runtime, config) and runs it as an isolated process on a shared operating system kernel. "Works on my machine" becomes "works on any machine with a container runtime."

## How it works

Containers are not small virtual machines. A **VM** emulates hardware and runs its own full OS kernel. A **container** is a normal Linux process that the kernel fences off using:

- **Namespaces**: give the process its own view of processes, network, mounts and hostnames.
- **cgroups**: cap how much CPU and memory it can use.
- **Layered filesystem**: the image is a stack of read-only layers plus a thin writable layer on top.

That's why containers start in under a second and you can run dozens on a laptop. On macOS and Windows, Docker Desktop runs a small Linux VM in the background to provide the Linux kernel.

## Key terms

- **Image**: the read-only template, built from a `Dockerfile`.
- **Container**: a running instance of an image.
- **Registry**: where images are stored and shared (Docker Hub, GitHub Container Registry).
- **Volume**: storage that outlives the container.
- **Orchestrator**: software that runs many containers across many machines and restarts failed ones (Kubernetes, ECS, Nomad).

## In practice

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

```bash
docker build -t myapp .
docker run -p 8000:8000 myapp
docker ps            # running containers
```

Copying `requirements.txt` and installing before copying the rest of the code means the slow dependency layer is reused when only your code changes.

## Common pitfalls

- Storing data inside the container's filesystem; it disappears when the container is removed. Use a volume.
- Huge images from full base images and leftover build tools. Use slim bases and multi-stage builds.
- Running as root inside the container when it isn't needed.

## Related

- [CI/CD](ci-cd.md)
- [Serverless](serverless.md)
