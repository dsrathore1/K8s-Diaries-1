<h1 align='center'>  Kubernetes Diaries: Building Scalable PostgreSQL from Scratch 🐳 </h1>

This project demonstrates how to deploy a scalable PostgreSQL database within a Kubernetes cluster created using KIND (Kubernetes IN Docker). It includes a FastAPI backend to monitor database health, automated backups to AWS S3, a React-based dashboard, and a CI/CD pipeline using GitHub Actions. All Kubernetes manifests are hand-written for full transparency and learning.

---

## Features

- PostgreSQL deployed in a KIND-based local Kubernetes cluster
- FastAPI backend exposes real-time database metrics: backups, disk usage, and active connections
- PostgreSQL backups automated using Kubernetes CronJobs and stored in AWS S3
- React dashboard displays database health in real time
- CI/CD pipeline via GitHub Actions automatically applies Kubernetes manifests on push
- Local development and testing using KIND; optional extension to cloud environments
- All Kubernetes resources are defined manually (no Helm)

---

## Tech Stack

- Kubernetes (KIND)
- PostgreSQL 15
- FastAPI
- React
- AWS S3
- GitHub Actions
- Docker
- kubectl

---

## Prerequisites

Ensure the following tools are installed on your machine:

- [Docker](https://www.docker.com/)
- [KIND](https://kind.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Python 3.9+](https://www.python.org/)
- [Node.js and npm](https://nodejs.org/)

---

## Project Setup (Local with KIND)

1. **Clone the repository**

```bash
git clone https://github.com/dsrathore1/kubernetes-diaries.git
cd kubernetes-diaries
````

2. **Create a KIND cluster**

```bash
kind create cluster --config=k8s/cluster/kind-config.yaml
```

3. **Apply Kubernetes manifests**

```bash
kubectl apply -f k8s/
```

4. **Deploy the FastAPI backend**

```bash
kubectl apply -f k8s/backend/
```

5. **Deploy the React frontend**

```bash
kubectl apply -f k8s/frontend/
```

6. **Port forward for local access**

```bash
kubectl port-forward svc/backend-service 8000:8000
kubectl port-forward svc/frontend-service 3000:3000
```

---

## Secrets Management for AWS S3

To enable the CronJob to push PostgreSQL backups to AWS S3, store AWS credentials in a Kubernetes secret:

```bash
kubectl create secret generic aws-s3-credentials \
  --from-literal=AWS_ACCESS_KEY_ID=your_access_key \
  --from-literal=AWS_SECRET_ACCESS_KEY=your_secret_key
```

These credentials are mounted into the CronJob container for use during backups.

---

## API Reference (FastAPI)

| Endpoint              | Method | Description                          |
| --------------------- | ------ | ------------------------------------ |
| `/status/backup`      | GET    | Returns the timestamp of last backup |
| `/status/disk`        | GET    | Shows current PostgreSQL disk usage  |
| `/status/connections` | GET    | Lists active database connections    |
| `/health`             | GET    | Application health check             |

Sample response:

```json
{
  "last_backup": "2025-07-20T03:15:00Z",
  "disk_usage": "1.3GB / 10GB",
  "connections": 8
}
```

---

## Dashboard Preview

The React-based frontend consumes the FastAPI endpoints to display:

* Time of the last successful backup
* Disk space usage
* Number of active connections

A screenshot or demo link can be added here for visual reference.

---

---

## GitHub Actions Workflow

The project includes a GitHub Actions workflow that performs the following on each push to the `main` branch:

* Lints Kubernetes manifests
* Builds and pushes Docker images for the backend and frontend to GitHub Container Registry
* Uses a GitHub Actions runner to apply the latest manifests to a KIND cluster
* Simulates production deployment for local or test environments

---

## Blog Series

This project is documented in a multi-part blog series for step-by-step learning and technical breakdown. [Kubernetes Diaries: Building Scalable PostgreSQL from Scratch](https://k8s-diaries.hashnode.dev/series/k8s-to-public-dashboard)

---

## Contribution Guidelines

Contributions are welcome. To contribute:

1. Fork the repository
2. Create a new branch (`feature/your-feature-name`)
3. Make your changes
4. Submit a pull request with a clear description of your additions

Please follow consistent code formatting and commit conventions.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
