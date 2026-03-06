

#### 1. How to write a Dockerfile for a Java app on Ubuntu?

```dockerfile
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY target/myapp.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

#### 2. What is the difference between CMD and ENTRYPOINT in Docker?


* `CMD` provides default arguments that can be overridden.
* `ENTRYPOINT` defines a fixed executable. Use `ENTRYPOINT` + `CMD` to allow arguments.

#### 3. What is the difference betwee COPY vs ADD in Docker?

* `COPY` only copies files/directories.
* `ADD` also handles remote URLs and archives (e.g., unzip `.tar.gz`).

#### 4. What is Docker tag and usage?

* `docker tag myimage myrepo/myapp:v1`
* Helps version images, e.g., `:latest`, `:v1`, `:prod`.


#### 5. Where do you store your Docker images? Do you use Docker Hub?

Docker images are stored in:

Docker Hub (public/private)

Amazon ECR (preferred for AWS integration)

GitHub Container Registry (optional)
