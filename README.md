# QuakeWatch

## Project Overview

QuakeWatch is a Python Flask web application that displays earthquake information through a simple dashboard.

This project demonstrates how to containerize a Flask application with Docker and deploy it to Kubernetes using production-style Kubernetes resources.

---

# Technologies Used

- Python 3.11
- Flask
- Docker
- Docker Compose
- Kubernetes
- Docker Desktop
- Matplotlib

---

# Project Structure

```text
QuakeWatch/
│
├── app.py
├── dashboard.py
├── utils.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── README.md
│
├── k8s/
│   ├── namespace.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── hpa.yaml
│   └── cronjob.yaml
│
├── templates/
└── static/
```

---

# Docker

## Build the Docker Image

```bash
docker build -t quakewatch .
```

## Run with Docker Compose

```bash
docker compose up --build
```

The application will be available at:

```text
http://localhost:5050
```

## Run the Docker Image

```bash
docker run -p 5050:5000 quakewatch
```

## Stop the Application

```bash
docker compose down
```

---

# Docker Hub

Docker Image:

```text
gh93/quakewatch:v1
```

Pull the image:

```bash
docker pull gh93/quakewatch:v1
```

---

# Kubernetes Deployment

Enable Kubernetes in Docker Desktop before deploying.

Deploy all Kubernetes resources:

```bash
kubectl apply -f k8s/
```

(Optional) Set the default namespace:

```bash
kubectl config set-context --current --namespace=quakewatch
```

Verify the deployment:

```bash
kubectl get pods
kubectl get svc
kubectl get deployment
```

---

# Access the Application

Expose the application locally:

```bash
kubectl port-forward svc/quakewatch-service 5000:80
```

Open:

```text
http://localhost:5000
```

---

# Kubernetes Resources

The project includes the following Kubernetes resources:

- Namespace
- ConfigMap
- Secret
- Deployment
- ReplicaSet (created automatically by the Deployment)
- Service (NodePort)
- Horizontal Pod Autoscaler (HPA)
- CronJob

---

# Monitoring

```bash
kubectl get pods
kubectl get svc
kubectl get hpa
kubectl top nodes
kubectl top pods
```

---

# CronJob

```bash
kubectl get cronjob
kubectl get jobs
kubectl logs job/<job-name>
```

---

# Author

George Hanna
