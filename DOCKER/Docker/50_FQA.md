
### **1. What is Docker?**

Docker is a containerization platform that automates application deployment in lightweight containers. Containers package the application code with all its dependencies, ensuring consistent performance across environments — development, testing, and production.

---

### **2. Difference between Virtual Machines and Containers.**

* **Virtual Machines:** Each VM runs a full OS with its own kernel on top of a hypervisor, which makes them heavy and resource-intensive.
* **Containers:** Share the host OS kernel, isolating only the user space. They are lightweight, start in seconds, and consume fewer resources.

---

### **3. What is a Docker Image?**

A Docker image is a read-only template containing the application and its dependencies. It’s built using a Dockerfile and used to create containers. Every image is made up of multiple layers stacked together.

---

### **4. What is a Docker Container?**

A container is a runtime instance of a Docker image. It runs the application in an isolated environment but uses the host kernel. Containers can be started, stopped, moved, and deleted easily without affecting the host.

---

### **5. What is a Dockerfile?**

A Dockerfile is a text file with instructions that define how to build a Docker image. Each instruction (e.g., `FROM`, `RUN`, `COPY`, `CMD`) creates a layer in the image.
Example:

```dockerfile
FROM python:3.9
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

---

### **6. What is Docker Hub?**

Docker Hub is a public cloud-based registry where Docker images are stored and shared. You can **pull** official images (like Ubuntu, Nginx, MySQL) or **push** your own images for others to use.

---

### **7. What are common Docker commands?**

| Action              | Command                          |
| ------------------- | -------------------------------- |
| List containers     | `docker ps`                      |
| List all containers | `docker ps -a`                   |
| Build image         | `docker build -t myapp .`        |
| Run container       | `docker run -d -p 8080:80 myapp` |
| Stop container      | `docker stop <id>`               |

---

### **8. What is the difference between `docker run` and `docker start`?**

* `docker run`: Creates a new container and starts it.
* `docker start`: Restarts an existing stopped container.

---

### **9. What is the use of `.dockerignore` file?**

It excludes files and directories (like `.git`, `node_modules`) from being copied into the image during the build, reducing build time and image size.

---

### **10. How to view logs of a container?**

```bash
docker logs <container_id>
```

It displays STDOUT and STDERR logs, which help in debugging runtime issues.

---


### **11. How to build a Docker image?**

```bash
docker build -t myimage:latest .
```

This reads the Dockerfile in the current directory and builds an image named `myimage`.

---

### **12. How to tag and push an image to Docker Hub?**

```bash
docker tag myimage:latest myrepo/myimage:v1
docker push myrepo/myimage:v1
```

Tagging helps identify versions and repositories clearly.

---

### **13. How to list and remove images?**

* List images: `docker images`
* Remove image: `docker rmi <image_id>`

---

### **14. What is the difference between `CMD` and `ENTRYPOINT`?**

* **CMD:** Sets default arguments for the container. It can be overridden during runtime.
* **ENTRYPOINT:** Defines the main command that always executes. CMD can pass arguments to ENTRYPOINT.

Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

---

### **15. What is a Docker Volume?**

A volume is a persistent data storage mechanism used by containers. It stores data outside the container’s writable layer, so data isn’t lost when containers are deleted.

---

### **16. How to create and attach a Docker volume?**

```bash
docker volume create mydata
docker run -v mydata:/app/data myimage
```

Here `/app/data` inside the container is mapped to `mydata` volume on the host.

---

### **17. How do you inspect container details?**

```bash
docker inspect <container_id>
```

It gives JSON output with detailed information — IP address, volumes, environment variables, etc.

---

### **18. How to enter into a running container?**

```bash
docker exec -it <container_id> /bin/bash
```

This provides an interactive terminal session inside the container for inspection or debugging.

---

### **19. What is a Docker network?**

Docker networks allow containers to communicate with each other or with external systems. Containers on the same network can communicate using container names instead of IPs.

---

### **20. What are types of Docker networks?**

* **Bridge:** Default; used for container-to-container communication on the same host.
* **Host:** Shares host network stack directly.
* **None:** Isolates containers from any network.
* **Overlay:** Used for multi-host communication in Swarm mode.
* **Macvlan:** Assigns a MAC address to the container, appearing as a physical device.

---

### **21. How to connect a container to an existing network?**

```bash
docker network connect mynetwork mycontainer
```

This attaches the container to a specific user-defined network.

---

### **22. What is a Multi-Stage Build?**

It allows using multiple `FROM` instructions in one Dockerfile. You can compile your app in one stage and copy only the required binaries into a final, smaller image — optimizing size and security.

---

### **23. How to view Docker image layers?**

```bash
docker history <image_name>
```

It shows all layers created from the Dockerfile instructions.

---

### **24. How do you handle environment variables in Docker?**

You can pass variables during container creation:

```bash
docker run -e ENV=prod myimage
```

Or use `ENV` instruction inside the Dockerfile.

---

### **25. How do you persist logs or data from a container?**

Use **volumes** or **bind mounts** to store data externally. Example:

```bash
docker run -v /host/logs:/var/log/nginx nginx
```

This ensures logs persist even if the container is removed.

---

### **26. What is Docker Compose?**

Docker Compose is a tool for defining and managing multi-container applications. It uses a `docker-compose.yml` file to configure services, networks, and volumes.

---

### **27. Example of a Docker Compose file:**

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
```

This defines two containers — Nginx and MySQL — that start together.

---

### **28. How to start and stop services in Compose?**

```bash
docker-compose up -d
docker-compose down
```

---

### **29. How do you scale services in Compose?**

```bash
docker-compose up --scale web=3 -d
```

This launches 3 replicas of the web service.

---

### **30. How to view running services in Compose?**

```bash
docker-compose ps
```

---
### **31. How to reduce Docker image size?**

* Use smaller base images like `alpine`.
* Combine `RUN` commands.
* Use multi-stage builds.
* Remove unnecessary packages and cache (`apt-get clean`).

---

### **32. How do you monitor container resource usage?**

```bash
docker stats
```

Displays real-time CPU, memory, network, and disk I/O metrics.

---

### **33. How do you clean up unused Docker resources?**

```bash
docker system prune -a
```

Removes stopped containers, unused images, and dangling volumes.

---

### **34. How to commit a running container as a new image?**

```bash
docker commit <container_id> mynewimage
```

It captures the current state of the container and saves it as an image.

---

### **35. How do you secure Docker containers?**

* Run containers as non-root users.
* Regularly scan images for vulnerabilities.
* Keep Docker and base images updated.
* Use minimal base images and limit exposed ports.

---

### **36. How to scan images for vulnerabilities?**

```bash
docker scan <image_name>
```

Or use third-party tools like **Trivy**, **Anchore**, or **Clair**.

---

### **37. What are Docker Secrets?**

Docker Secrets are used to securely store and manage sensitive data like passwords or tokens. They are encrypted at rest and in transit (mainly used in Swarm mode).

---

### **38. How is Docker used in CI/CD pipelines?**

In CI/CD, Docker ensures consistency between build, test, and deployment environments. Tools like **Jenkins** or **GitLab CI** build Docker images, run tests in containers, and deploy the same image to production.

---

### **39. How do you integrate Docker with Jenkins?**

* Use Docker agent inside Jenkins pipelines.
* Build images using `docker build` steps.
* Push images to a registry and deploy using scripts.

Example (Jenkinsfile):

```groovy
agent { docker { image 'node:14' } }
stages {
  stage('Build') {
    steps { sh 'npm install' }
  }
}
```

---

### **40. What is Docker Swarm?**

Docker Swarm is Docker’s native clustering and orchestration tool that allows you to manage multiple containers across different hosts as a single virtual system.

---

### **41. Difference between Docker Swarm and Kubernetes.**

| Feature        | Docker Swarm | Kubernetes           |
| -------------- | ------------ | -------------------- |
| Setup          | Simple       | Complex              |
| Scaling        | Quick        | Granular             |
| Load Balancing | Built-in     | Built-in             |
| Community      | Smaller      | Larger & widely used |

---

### **42. How do you perform rolling updates in Docker Swarm?**

Using the command:

```bash
docker service update --image newimage:tag myservice
```

This replaces old containers one by one, ensuring zero downtime.

---

### **43. How do you troubleshoot a container that fails to start?**

* Check logs using `docker logs <id>`.
* Inspect the container: `docker inspect <id>`.
* Validate image build using `docker build --no-cache`.
* Ensure correct port and environment variable setup.

---

### **44. How to limit CPU and memory usage of a container?**

```bash
docker run --cpus="1" --memory="512m" myimage
```

This restricts container resources to prevent system overuse.

---

### **45. How to check Docker version and info?**

```bash
docker version
docker info
```

`docker info` provides detailed configuration and runtime data.

---

### **46. What is an overlay network in Docker?**

An overlay network connects multiple Docker daemons together, allowing containers running on different hosts to communicate securely. It's primarily used in Swarm mode.

---

### **47. How to view Docker disk usage?**

```bash
docker system df
```

Shows how much space images, containers, and volumes occupy.

---

### **48. How does Docker handle image caching during builds?**

Docker caches layers from previous builds to speed up future builds. If a command or file changes, caching breaks from that layer onward.

---

### **49. How do you troubleshoot slow Docker builds?**

* Use `.dockerignore` to exclude unnecessary files.
* Combine `RUN` steps to reduce layers.
* Reorder Dockerfile commands to maximize cache hits.

---

### **50. What are some Docker best practices for DevOps engineers?**

* Use small, minimal images.
* Don’t store credentials in Dockerfiles.
* Use health checks to monitor container health.
* Employ CI/CD pipelines to automate image builds and deployment.
* Regularly scan and update images.
