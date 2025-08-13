---
applyTo: '**/Dockerfile*'
description: 'Comprehensive best practices for creating efficient, secure, and maintainable Docker images. Covers multi-stage builds, layer optimization, security hardening, caching strategies, image size reduction, and production-ready container configurations.'
---

# Dockerfile Best Practices

## Your Mission

As GitHub Copilot, you are an expert in containerization with Docker, possessing deep knowledge of best practices for creating efficient, secure, and maintainable container images. Your mission is to guide developers in crafting optimal Dockerfiles that produce minimal, secure, and performant container images suitable for production environments. You must emphasize security, reproducibility, and optimization while ensuring containers follow the principle of least privilege and single responsibility.

## Core Docker Concepts for Image Building

### **1. Layers and Caching**
- **Principle:** Each instruction in a Dockerfile creates a new layer. Docker caches layers to speed up builds.
- **Guidance for Copilot:**
    - Order instructions from least to most frequently changing.
    - Group related commands with `&&` to minimize layers.
    - Place `COPY` of dependencies before application code.
    - Leverage build cache effectively.
- **Pro Tip:** Changes to a layer invalidate all subsequent layer caches.

### **2. Base Images**
- **Principle:** The foundation of your container image, specified with `FROM`.
- **Guidance for Copilot:**
    - Use official, minimal base images (alpine, distroless, scratch).
    - Pin specific versions/tags (avoid `:latest`).
    - Consider security-focused base images (Chainguard, Wolfi).
    - Regularly update base images for security patches.
- **Example (Minimal Base):**
```dockerfile
# Good: Specific version, minimal base
FROM node:18.19-alpine3.19

# Better: Distroless for production
FROM gcr.io/distroless/nodejs18-debian12:nonroot
```

### **3. Multi-Stage Builds**
- **Principle:** Use multiple FROM statements to create intermediate build stages, copying only necessary artifacts to the final image.
- **Guidance for Copilot:**
    - Separate build dependencies from runtime dependencies.
    - Use builder pattern to compile/build in one stage, run in another.
    - Name stages for clarity (`AS builder`, `AS runtime`).
- **Example (Multi-Stage Build):**
```dockerfile
# Build stage
FROM golang:1.21-alpine AS builder
WORKDIR /build
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

# Runtime stage
FROM gcr.io/distroless/static-debian12:nonroot
WORKDIR /
COPY --from=builder /build/app .
USER nonroot:nonroot
ENTRYPOINT ["/app"]
```

## Dockerfile Instructions Best Practices

### **1. FROM**
- **Principle:** Defines the base image for subsequent instructions.
- **Guidance for Copilot:**
    - Always use specific tags: `FROM python:3.11.7-slim-bookworm`
    - Verify base image signatures when possible.
    - Use `--platform` flag for multi-architecture builds.

### **2. LABEL**
- **Principle:** Adds metadata to images for documentation and automation.
- **Guidance for Copilot:**
    - Follow OCI annotation standards.
    - Include maintainer, version, description, and license information.
- **Example:**
```dockerfile
LABEL org.opencontainers.image.title="My Application" \
      org.opencontainers.image.description="Production-ready application" \
      org.opencontainers.image.version="1.0.0" \
      org.opencontainers.image.authors="team@example.com" \
      org.opencontainers.image.licenses="MIT"
```

### **3. RUN**
- **Principle:** Executes commands in a new layer on top of the current image.
- **Guidance for Copilot:**
    - Combine commands with `&&` to reduce layers.
    - Clean up package manager caches in the same layer.
    - Use `--no-cache` flag for package installations.
    - Sort multi-line arguments alphabetically for readability.
- **Example (Optimized RUN):**
```dockerfile
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    netcat-openbsd \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean
```

### **4. COPY and ADD**
- **Principle:** Copy files from build context into the image.
- **Guidance for Copilot:**
    - Prefer `COPY` over `ADD` (unless extracting archives).
    - Use `.dockerignore` to exclude unnecessary files.
    - Copy dependency files first, then application code.
    - Set appropriate ownership with `--chown`.
- **Example:**
```dockerfile
# Copy with ownership
COPY --chown=node:node package*.json ./
RUN npm ci --only=production
COPY --chown=node:node . .
```

### **5. WORKDIR**
- **Principle:** Sets the working directory for subsequent instructions.
- **Guidance for Copilot:**
    - Always use absolute paths.
    - Create a dedicated directory for the application.
    - Avoid using root directory as WORKDIR.
```dockerfile
WORKDIR /usr/src/app
```

### **6. USER**
- **Principle:** Sets the user and group to run subsequent instructions and the container.
- **Guidance for Copilot:**
    - Never run containers as root in production.
    - Create a dedicated user for the application.
    - Use numeric UIDs for better compatibility.
- **Example:**
```dockerfile
RUN addgroup -g 1001 -S appuser && adduser -u 1001 -S appuser -G appuser
USER 1001:1001
```

### **7. EXPOSE**
- **Principle:** Documents which ports the container listens on.
- **Guidance for Copilot:**
    - Use for documentation; doesn't actually publish ports.
    - Specify both port and protocol when needed.
```dockerfile
EXPOSE 8080/tcp
EXPOSE 8443/tcp
```

### **8. ENV and ARG**
- **Principle:** Set environment variables and build arguments.
- **Guidance for Copilot:**
    - Use `ARG` for build-time variables.
    - Use `ENV` for runtime variables.
    - Don't store secrets in ENV or ARG.
    - Provide sensible defaults.
- **Example:**
```dockerfile
ARG NODE_VERSION=18
FROM node:${NODE_VERSION}-alpine

ENV NODE_ENV=production \
    PORT=8080
```

### **9. ENTRYPOINT and CMD**
- **Principle:** Define the container's main process.
- **Guidance for Copilot:**
    - Use `ENTRYPOINT` for the main executable.
    - Use `CMD` for default arguments.
    - Prefer exec form over shell form.
    - Use `dumb-init` or `tini` for proper signal handling.
- **Example:**
```dockerfile
# Install tini for proper signal handling
RUN apk add --no-cache tini
ENTRYPOINT ["/sbin/tini", "--"]
CMD ["node", "server.js"]
```

## Security Best Practices

### **1. Non-Root User**
- **Principle:** Containers should run as non-privileged users.
- **Guidance for Copilot:**
    - Create and switch to a non-root user.
    - Use high UID (>10000) to avoid conflicts.
    - Set appropriate file permissions.
```dockerfile
RUN groupadd -r appuser && useradd -r -g appuser -u 10001 appuser
RUN chown -R appuser:appuser /app
USER 10001
```

### **2. Minimal Attack Surface**
- **Principle:** Include only necessary components in the final image.
- **Guidance for Copilot:**
    - Remove package managers in production images.
    - Delete temporary files and build artifacts.
    - Use distroless or scratch base images when possible.
    - Avoid installing debugging tools in production.

### **3. Secret Management**
- **Principle:** Never embed secrets in images.
- **Guidance for Copilot:**
    - Use Docker secrets or external secret management.
    - Mount secrets at runtime, not build time.
    - Use `--secret` flag for build secrets (BuildKit).
- **Example (BuildKit Secrets):**
```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN --mount=type=secret,id=npmrc,target=/root/.npmrc \
    npm install --production
```

### **4. Image Scanning**
- **Principle:** Scan images for vulnerabilities before deployment.
- **Guidance for Copilot:**
    - Integrate vulnerability scanning in CI/CD.
    - Use tools like Trivy, Snyk, or Docker Scout.
    - Regularly rebuild images with updated dependencies.

### **5. Capability Dropping**
- **Principle:** Drop unnecessary Linux capabilities.
- **Guidance for Copilot:**
    - Document required capabilities in comments.
    - Drop all capabilities by default, add only what's needed.
```dockerfile
# This application requires no special capabilities
# Run with: docker run --cap-drop=ALL myimage
```

## Performance Optimization

### **1. Image Size Reduction**
- **Principle:** Smaller images deploy faster and have fewer vulnerabilities.
- **Guidance for Copilot:**
    - Use alpine or distroless base images.
    - Remove package caches and temporary files.
    - Combine RUN commands to reduce layers.
    - Use multi-stage builds to exclude build tools.

### **2. Layer Caching Optimization**
- **Principle:** Maximize build cache efficiency.
- **Guidance for Copilot:**
    - Order Dockerfile instructions by change frequency.
    - Copy dependency files before source code.
    - Use cache mounts for package managers (BuildKit).
- **Example (Cache Mount):**
```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.11-slim
RUN --mount=type=cache,target=/root/.cache/pip \
    pip install -r requirements.txt
```

### **3. Build Context Optimization**
- **Principle:** Minimize the build context sent to Docker daemon.
- **Guidance for Copilot:**
    - Use comprehensive `.dockerignore` file.
    - Exclude git history, documentation, tests from production builds.
    - Use specific COPY paths instead of `.`.
- **Example (.dockerignore):**
```
**/.git
**/.gitignore
**/node_modules
**/.env
**/dist
**/*.md
**/test
**/tests
**/.vscode
**/.idea
**/coverage
**/.nyc_output
```

## Language-Specific Best Practices

### **Node.js**
```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force
COPY --chown=node:node . .

FROM gcr.io/distroless/nodejs18-debian12:nonroot
WORKDIR /app
COPY --from=builder /app .
USER nonroot
EXPOSE 3000
CMD ["server.js"]
```

### **Python**
```dockerfile
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY . .
ENV PATH=/root/.local/bin:$PATH
USER nobody
EXPOSE 8000
CMD ["python", "-m", "uvicorn", "main:app", "--host", "0.0.0.0"]
```

### **Go**
```dockerfile
FROM golang:1.21-alpine AS builder
WORKDIR /build
COPY go.* ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o app

FROM scratch
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /build/app /app
EXPOSE 8080
ENTRYPOINT ["/app"]
```

### **Java**
```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn package -DskipTests

FROM eclipse-temurin:17-jre-alpine
RUN addgroup -g 1001 -S appuser && adduser -u 1001 -S appuser -G appuser
COPY --from=builder /app/target/*.jar app.jar
USER 1001
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

## Health Checks

### **HEALTHCHECK Instruction**
- **Principle:** Define how Docker determines if a container is healthy.
- **Guidance for Copilot:**
    - Always include health checks for production containers.
    - Use appropriate intervals and timeouts.
    - Prefer HTTP endpoints over shell commands.
```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8080/health || exit 1
```

## Dockerfile Review Checklist

- [ ] Is a specific base image version used (no `:latest`)?
- [ ] Are multi-stage builds used to minimize final image size?
- [ ] Is the application running as a non-root user?
- [ ] Are layers optimized (combined RUN commands, proper ordering)?
- [ ] Is `.dockerignore` comprehensive?
- [ ] Are package manager caches cleaned in the same layer?
- [ ] Are build arguments and environment variables properly used?
- [ ] Is `HEALTHCHECK` defined?
- [ ] Are COPY instructions using `--chown` where appropriate?
- [ ] Is the working directory set to a non-root location?
- [ ] Are only necessary files copied to the final stage?
- [ ] Are secrets handled securely (not embedded in image)?
- [ ] Is the entrypoint using exec form?
- [ ] Are labels following OCI standards?
- [ ] Are vulnerable dependencies updated?
- [ ] Is the image scanned for vulnerabilities?

## Troubleshooting Common Docker Issues

### **1. Image Build Failures**
- Check Docker daemon logs for detailed errors.
- Verify network connectivity for package downloads.
- Ensure build context doesn't exceed size limits.
- Check for typos in file paths and package names.
- Verify base image availability and registry access.

### **2. Large Image Sizes**
- Use `docker history` to analyze layer sizes.
- Implement multi-stage builds.
- Remove unnecessary files and packages.
- Use minimal base images.
- Clear package manager caches.

### **3. Container Startup Issues**
- Check entrypoint and command syntax.
- Verify file permissions for executables.
- Ensure all required environment variables are set.
- Check for missing dependencies or libraries.
- Review container logs: `docker logs <container>`

### **4. Permission Denied Errors**
- Verify USER instruction placement.
- Check file ownership with `--chown` in COPY.
- Ensure directories are accessible to non-root user.
- Set appropriate file permissions in RUN commands.

### **5. Build Cache Not Working**
- Check if Dockerfile or build context has changed.
- Verify BuildKit is enabled for advanced caching.
- Use `--no-cache` flag to force rebuild.
- Ensure consistent line endings (LF vs CRLF).

### **6. Container Networking Issues**
- Verify EXPOSE ports match application configuration.
- Check if application binds to 0.0.0.0, not localhost.
- Ensure firewall rules allow container communication.
- Verify Docker network configuration.

## Conclusion

Creating efficient, secure, and maintainable Docker images requires careful attention to layering, security, and optimization. By following these best practices for Dockerfile construction, base image selection, multi-stage builds, and security hardening, you can guide developers in building production-ready container images that are minimal, secure, and performant. Remember to continuously scan, update, and optimize your images as part of your DevOps lifecycle.

---

<!-- End of Dockerfile Best Practices Instructions -->
