# Docker Deployment Guide

## Overview
This React application has been dockerized using both **single-stage** and **multi-stage** Docker builds.

## Files Created

### 1. `.dockerignore`
Excludes unnecessary files from the Docker build context:
- node_modules
- build artifacts
- git files
- documentation

### 2. `Dockerfile.single` (Single-Stage Build)
- **Base Image**: node:18-alpine
- **Final Size**: ~638 MB
- **Description**: Contains all build dependencies and tools
- **Use Case**: Development, debugging

### 3. `Dockerfile.multi` (Multi-Stage Build)
- **Build Stage**: node:18-alpine
- **Production Stage**: nginx:alpine  
- **Final Size**: ~53 MB (92% smaller!)
- **Description**: Only contains production-ready built files
- **Use Case**: Production deployment (Recommended)

## Images Built

### Single-Stage Image
```bash
dheerajgoud353/react-app-single:latest
```
- Size: 638 MB
- Image ID: ae2b6ebebb82

### Multi-Stage Image (Recommended)
```bash
dheerajgoud353/react-app-multi:latest
```
- Size: 53.3 MB  
- Image ID: 8befff55a396

## Build Commands

### Build Single-Stage Image
```bash
docker build -f Dockerfile.single -t dheerajgoud353/react-app-single:latest .
```

### Build Multi-Stage Image
```bash
docker build -f Dockerfile.multi -t dheerajgoud353/react-app-multi:latest .
```

## Running Locally

### Run Single-Stage Container
```bash
docker run -p 3000:3000 dheerajgoud353/react-app-single:latest
```
Access at: http://localhost:3000

### Run Multi-Stage Container
```bash
docker run -p 8080:80 dheerajgoud353/react-app-multi:latest
```
Access at: http://localhost:8080

## Pushing to Docker Hub

### Prerequisites
1. Create a Docker Hub account at https://hub.docker.com
2. Create an access token at https://hub.docker.com/settings/security

### Login to Docker Hub
```bash
docker login -u dheerajgoud353
# Enter your Docker Hub password or access token when prompted
```

### Push Single-Stage Image
```bash
docker push dheerajgoud353/react-app-single:latest
```

### Push Multi-Stage Image
```bash
docker push dheerajgoud353/react-app-multi:latest
```

## Alternative: GitHub Container Registry

Images have also been tagged for GitHub Container Registry:

```bash
# Tag for GHCR (already done)
docker tag dheerajgoud353/react-app-single:latest ghcr.io/dheerajgoud353/react-app-single:latest
docker tag dheerajgoud353/react-app-multi:latest ghcr.io/dheerajgoud353/react-app-multi:latest

# Login to GHCR
echo $GITHUB_TOKEN | docker login ghcr.io -u dheerajgoud353 --password-stdin

# Push to GHCR
docker push ghcr.io/dheerajgoud353/react-app-single:latest
docker push ghcr.io/dheerajgoud353/react-app-multi:latest
```

## Comparison: Single vs Multi-Stage

| Feature | Single-Stage | Multi-Stage |
|---------|--------------|-------------|
| Size | 638 MB | 53.3 MB |
| Build Tools Included | ✅ Yes | ❌ No |
| Production Ready | ⚠️ Not optimal | ✅ Yes |
| Web Server | serve (Node.js) | nginx |
| Port | 3000 | 80 |
| Security | Lower | Higher |
| Performance | Slower | Faster |
| **Recommendation** | Development | **Production** |

## Key Advantages of Multi-Stage Build

1. **92% Smaller Size**: 53MB vs 638MB
2. **Faster Deployment**: Smaller images = faster push/pull
3. **Better Security**: No build tools in production
4. **Optimized Performance**: nginx is faster than Node.js serve
5. **Lower Attack Surface**: Fewer dependencies = fewer vulnerabilities

## Docker Hub Repository

Once pushed, your images will be available at:
- https://hub.docker.com/r/dheerajgoud353/react-app-single
- https://hub.docker.com/r/dheerajgoud353/react-app-multi

## Pull Commands (After Pushing)

```bash
# Pull single-stage image
docker pull dheerajgoud353/react-app-single:latest

# Pull multi-stage image
docker pull dheerajgoud353/react-app-multi:latest
```

## Dockerfile Best Practices Implemented

✅ Multi-stage builds for smaller images
✅ .dockerignore to exclude unnecessary files
✅ Alpine-based images for minimal size
✅ Layer caching optimization
✅ Non-root user in nginx
✅ Production-grade web server (nginx)

## Next Steps

1. **Login to Docker Hub** with your credentials
2. **Push both images** using the commands above  
3. **Verify** images on Docker Hub website
4. **Pull and run** from any machine with Docker installed
5. **Deploy** to production (Kubernetes, AWS ECS, etc.)

---

**Created**: November 6, 2025
**Docker Version**: 28.3.1-1
**Node Version**: 18-alpine
**Nginx Version**: alpine
