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


## CI/CD Pipeline
The GitHub Actions workflow automatically triggers on every push to the 
main branch. It:
1. Checks out the code
2. Logs into GitHub Container Registry (GHCR)
3. Builds the Docker image
4. Pushes the image to GHCR
5. Updates the deployment.yaml with the new image tag
6. Commits the updated deployment back to the repository