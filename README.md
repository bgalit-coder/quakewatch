# QuakeWatch – Dockerized Flask Application

This repository contains a Dockerized version of the QuakeWatch Flask application.

The goal of this project is to containerize the existing application using Docker and Docker Compose, according to the requirements of Phase 1 in the DevOps Engineer Course.

The original application source code and its documentation remain unchanged inside the `QuakeWatch/` directory.

For application functionality, features, and internal details, refer to:
`QuakeWatch/README.md`

---

## Project Structure

quakewatch-devops/
├── Dockerfile
├── docker-compose.yml
├── README.md
└── QuakeWatch/
    ├── app.py
    ├── dashboard.py
    ├── utils.py
    ├── requirements.txt
    ├── static/
    └── templates/

---

## Prerequisites

Docker installed and running  
Docker Compose installed

Verify installation:

docker --version  
docker-compose --version

---

## Build the Docker Image

From the project root directory:

docker build -t quakewatch:latest .

Verify the image was created:

docker images | grep quakewatch

---

## Run the Application with Docker

Run the container:

docker run --rm -p 5000:5000 quakewatch:latest

Access the application in a browser:

http://localhost:5000

Stop the application by pressing Ctrl+C.

---

## Run the Application with Docker Compose

Start the application:

docker-compose up --build

Access the application in a browser:

http://localhost:5000

Stop and remove the containers:

docker-compose down

---

## Deliverables

Dockerfile for building the image  
docker-compose.yml for running the application  
Docker image built locally  
This README file describing how to build and run the containerized application

---

## Phase 2 – Kubernetes (Docker Desktop)

### Prerequisites
- Docker Desktop with Kubernetes enabled
- kubectl installed and working

### Build Docker image (Phase 1)
From the project root (where the Dockerfile is):
docker build -t quakewatch:1.0 .
docker images | grep quakewatch

### Deploy to Kubernetes (Phase 2)
Apply manifests:
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secret.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/hpa.yaml
kubectl apply -f k8s/cronjob-healthcheck.yaml

### Verify resources
kubectl -n quakewatch get deployments,pods,svc,cronjob,hpa

### Access the application
Using port-forward (local access):
kubectl -n quakewatch port-forward svc/quakewatch-svc 8001:8000

Open in browser:
http://localhost:8001

Note:
- The Service port is 8000 inside the cluster.
- Local port-forward may use a different port (e.g., 8001) if 8000 is already in use on the machine.

### CronJob internal health check
To verify CronJob is running:
kubectl -n quakewatch get cronjob
kubectl -n quakewatch get jobs
kubectl -n quakewatch logs job/<JOB_NAME>

### HPA
An HPA manifest is included. Active autoscaling may require metrics-server in the cluster.


