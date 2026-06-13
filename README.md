# Wisecow Application

## Overview
Wisecow is a web application that serves a random quote via an ASCII cow 
every time the user visits the browser. It is containerized using Docker 
and deployed on Kubernetes with TLS encryption and an automated CI/CD pipeline.

## Project Structure

PS1-wisecow/

├── wisecow/

│   ├── wisecow.sh

│   └── Dockerfile

├── k8s/

│   ├── deployment.yaml

│   ├── service.yaml

│   └── ingress.yaml

├── .github/

│   └── workflows/

│       └── deploy.yml

└── README.md

## Prerequisites
- Docker
- Minikube
- kubectl
- openssl

## Setup and Deployment

### 1. Clone the repository
```bash
git clone https://github.com/Aishwini08/PS1-wisecow.git
cd PS1-wisecow
```

### 2. Build Docker image
```bash
docker build -t wisecow:latest .
```

### 3. Load image into Minikube
```bash
minikube image load wisecow:latest
```

### 4. Deploy to Kubernetes
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

### 5. TLS Setup
```bash
# Generate self-signed certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key -out tls.crt \
  -subj "/CN=wisecow.local/O=wisecow"

# Create Kubernetes TLS secret
kubectl create secret tls wisecow-tls --cert=tls.crt --key=tls.key
```

### 6. Enable Ingress
```bash
minikube addons enable ingress
```

### 7. Access the Application
```bash
# Start minikube tunnel
minikube tunnel
```

Add this to your hosts file (`C:\Windows\System32\drivers\etc\hosts`):


## CI/CD Pipeline
The GitHub Actions workflow automatically triggers on every push to the 
main branch. It:
1. Checks out the code
2. Logs into GitHub Container Registry (GHCR)
3. Builds the Docker image
4. Pushes the image to GHCR
5. Updates the deployment.yaml with the new image tag
6. Commits the updated deployment back to the repository