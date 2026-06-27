# QuakeWatch

## Project Overview

QuakeWatch is a Python Flask web application that displays earthquake information through a simple dashboard.

This project demonstrates a complete DevOps workflow, including:

- Containerizing the application with Docker
- Running the application with Docker Compose
- Deploying the application to Kubernetes
- Managing Kubernetes resources using Helm
- Publishing Docker images and Helm charts to Docker Hub
- Automating CI/CD with GitHub Actions
- Deploying declaratively with ArgoCD using GitOps

---

## Technologies Used

- Python 3.11
- Flask
- Docker
- Docker Compose
- Docker Hub
- Kubernetes
- Docker Desktop
- Helm
- GitHub Actions
- ArgoCD
- Matplotlib

---

## Project Structure

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
├── charts/
│   └── quakewatch/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── cd.yml
│
├── templates/
└── static/
```

---

## Docker

### Build the Docker Image

```bash
docker build -t quakewatch .
```

### Run with Docker Compose

```bash
docker compose up --build
```

The application will be available at:

```text
http://localhost:5050
```

### Run with Docker Directly

```bash
docker run -p 5050:5000 quakewatch
```

### Stop Containers

```bash
docker compose down
```

---

## Docker Hub

Docker image:

```text
gh93/quakewatch:v1
```

Pull the image:

```bash
docker pull gh93/quakewatch:v1
```

---

## Kubernetes Deployment

Enable Kubernetes in Docker Desktop before deploying.

### Deploy All Kubernetes Resources

```bash
kubectl apply -f k8s/
```

### Set the Namespace

```bash
kubectl config set-context --current --namespace=quakewatch
```

### Verify the Deployment

```bash
kubectl get pods
kubectl get svc
kubectl get deployment
```

---

## Access the Application

Expose the application locally:

```bash
kubectl port-forward svc/quakewatch-service 5000:80
```

Open:

```text
http://localhost:5000
```

---

## Kubernetes Resources

The project includes the following Kubernetes resources:

- Namespace
- ConfigMap
- Secret
- Deployment
- ReplicaSet
- Service
- Horizontal Pod Autoscaler (HPA)
- CronJob

---

## Health Checks

The Deployment includes:

- Readiness Probe
- Liveness Probe

These probes allow Kubernetes to check whether the application is ready to receive traffic and whether it should be restarted.

---

## Horizontal Pod Autoscaler

The HPA scales the application based on CPU usage.

Check the HPA:

```bash
kubectl get hpa
```

View resource usage:

```bash
kubectl top nodes
kubectl top pods
```

---

## CronJob

The CronJob runs every minute and checks whether the QuakeWatch Service is reachable from inside the Kubernetes cluster.

Check the CronJob:

```bash
kubectl get cronjob
```

Check created Jobs:

```bash
kubectl get jobs
```

View Job logs:

```bash
kubectl logs job/<job-name>
```

---

## Helm

The Kubernetes manifests were packaged as a Helm chart.

### Render the Chart

```bash
helm template quakewatch charts/quakewatch
```

### Install the Chart Locally

```bash
helm install quakewatch charts/quakewatch -n quakewatch
```

### List Helm Releases

```bash
helm list -n quakewatch
```

### Package the Chart

```bash
helm package charts/quakewatch
```

### Publish the Chart to Docker Hub

```bash
helm registry login registry-1.docker.io
helm push quakewatch-0.1.0.tgz oci://registry-1.docker.io/gh93
```

### Pull the Published Chart

```bash
helm pull oci://registry-1.docker.io/gh93/quakewatch --version 0.1.0
```

---

## GitHub Actions

The project includes two GitHub Actions workflows.

### CI Workflow

The CI workflow runs on pushes to non-main branches.

It performs:

- Code checkout
- Python setup
- Dependency installation
- Pylint check
- Docker image build

### CD Workflow

The CD workflow runs when changes are pushed or merged into the main branch.

It performs:

- Code checkout
- Docker Hub login
- Docker image build
- Docker image push to Docker Hub

Required GitHub repository secrets:

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

---

## ArgoCD GitOps

ArgoCD is used to deploy the Helm chart from the Git repository.

The ArgoCD Application points to:

```text
charts/quakewatch
```

GitOps flow:

```text
Git change
↓
GitHub repository update
↓
ArgoCD detects the change
↓
Kubernetes is synchronized automatically
```

This means the application can be updated by changing the Helm chart in Git and pushing the change, without manually running `kubectl apply`.

---

## Useful Commands

```bash
kubectl get pods
kubectl get svc
kubectl get deployment
kubectl get hpa
kubectl get cronjob
kubectl get jobs
kubectl top nodes
kubectl top pods
helm list -n quakewatch
```

---

## Author

George Hanna
