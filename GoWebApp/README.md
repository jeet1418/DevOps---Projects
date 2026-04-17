# Go Web Application

This is a simple website written in Golang. It uses the `net/http` package to serve HTTP requests.

## Running the server

To run the server, execute the following command:

```bash
go run main.go
```

The server will start on port 8080. You can access it by navigating to `http://localhost:8080/courses` in your web browser.

## Looks like this

![Website](static/images/golang-website.png)

## Deployment on AWS EKS

This section documents deploying the Go web application using Docker, Kubernetes (EKS), and NGINX Ingress on AWS.

### Architecture

Browser → Custom Domain → AWS ELB (Ingress) → NGINX Ingress → Kubernetes Service → Go Pod

### Issues & Solutions

#### CPU Architecture Mismatch (ARM vs AMD64)

**Problem**: Docker image built on Mac (Apple Silicon) failed on AWS EKS cluster.

**Symptoms**:

- ImagePullBackOff errors
- `no match for platform in manifest`
- `exec format error`

**Root Cause**:
| Component | Architecture |
|-----------|--------------|
| Mac (M1/M2/M3) | ARM64 |
| AWS EKS Nodes | AMD64 |

**Solution**:

Rebuild the Docker image for the correct architecture:


docker buildx build \
    --platform linux/amd64 \
    -t <docker-user>/go-web-app:v4 \
    --push .