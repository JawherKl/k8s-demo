# Kubernetes Demo Project

![Repository Size](https://img.shields.io/github/repo-size/JawherKl/k8s-demo)
![Last Commit](https://img.shields.io/github/last-commit/JawherKl/k8s-demo)
![Issues](https://img.shields.io/github/issues-raw/JawherKl/k8s-demo)
![Forks](https://img.shields.io/github/forks/JawherKl/k8s-demo)
![Stars](https://img.shields.io/github/stars/JawherKl/k8s-demo)

![nodepost](https://github.com/JawherKl/k8s-demo/blob/main/kubernetes.jpg?raw=true)

This repository contains a demonstration of deploying and managing applications using **Kubernetes (k8s)**. It is designed to help users understand the basics of Kubernetes, including deployments, services, scaling, and other core concepts. Whether you're new to Kubernetes or looking for a practical example to experiment with, this project provides a hands-on approach to working with k8s.

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Getting Started](#getting-started)
4. [Project Structure](#project-structure)
5. [Deploying the Application](#deploying-the-application)
6. [Scaling and Managing the Application](#scaling-and-managing-the-application)
7. [Cleaning Up](#cleaning-up)
8. [Contributing](#contributing)
9. [License](#license)

---

## Overview

This project demonstrates how to:
- Deploy a sample application to a Kubernetes cluster.
- Expose the application using Kubernetes Services.
- Scale the application using ReplicaSets.
- Manage configurations using ConfigMaps and Secrets.
- Clean up resources after deployment.
- Implement GitOps practices using ArgoCD
- Manage multiple environments (dev, staging, prod)
- Automate deployments using Helm charts

The demo application is a simple web server that serves a static webpage or API endpoint.

---

## Prerequisites

Before you begin, ensure you have the following installed:

- **Kubernetes Cluster**: You can use a local cluster like [Minikube](https://minikube.sigs.k8s.io/docs/), [Kind](https://kind.sigs.k8s.io/), or a cloud-based cluster (e.g., GKE, EKS, AKS).
- **kubectl**: The Kubernetes command-line tool. [Installation guide](https://kubernetes.io/docs/tasks/tools/).
- **Docker**: To build and manage container images. [Installation guide](https://docs.docker.com/get-docker/).

---

## Getting Started

1. Clone this repository:
   ```bash
   git clone https://github.com/JawherKl/k8s-demo.git
   cd k8s-demo
   ```

2. Ensure your Kubernetes cluster is running:
   ```bash
   kubectl cluster-info
   ```

3. Build the Docker image (if applicable):
   ```bash
   docker build -t your-username/k8s-demo-app:latest .
   ```

4. Deploy the application to your cluster (see [Deploying the Application](#deploying-the-application)).

---

## Project Structure

```
k8s-demo/
├── manifests/               # Kubernetes YAML files
│   ├── deployment.yaml      # Deployment configuration
│   ├── service.yaml        # Service configuration
│   └── configmap.yaml      # ConfigMap for environment variables
├── helm/                   # Helm chart directory
│   └── webapp/            
│       ├── Chart.yaml      # Helm chart definition
│       ├── values.yaml     # Default values
│       ├── values-dev.yaml # Development values
│       └── templates/      # Helm templates
├── argocd/                 # ArgoCD configurations
│   └── projects/
│       └── webapp.yaml     # ArgoCD application definitions
├── src/                    # Application source code
├── Dockerfile              # Dockerfile for the application
└── README.md              # This file
```

---

## Deploying the Application

1. Apply the Kubernetes manifests:
   ```bash
   kubectl apply -f manifests/
   ```

2. Verify the deployment:
   ```bash
   kubectl get pods
   kubectl get services
   ```

3. Access the application:
   - If using a local cluster (e.g., Minikube), run:
     ```bash
     minikube service <service-name>
     ```
   - For cloud clusters, use the external IP or LoadBalancer endpoint provided by the service.

---

## Scaling and Managing the Application

- Scale the application:
  ```bash
  kubectl scale deployment <deployment-name> --replicas=3
  ```

- Update the application:
  - Modify the `deployment.yaml` file or other manifests.
  - Reapply the changes:
    ```bash
    kubectl apply -f manifests/
    ```

- View logs:
  ```bash
  kubectl logs <pod-name>
  ```

---

## Cleaning Up

To delete the deployed resources:
```bash
kubectl delete -f manifests/
```

---

## Helm Implementation

This project uses Helm for package management and deployment. The Helm chart structure is organized as follows:

### Chart Structure
```
helm/webapp/
├── Chart.yaml          # Chart metadata
├── values.yaml         # Default values
├── values-dev.yaml     # Development values
├── values-staging.yaml # Staging values
├── values-prod.yaml    # Production values
└── templates/          # Helm templates
    ├── deployment.yaml
    ├── service.yaml
    ├── ingress.yaml
    ├── configmap.yaml
    ├── secret.yaml
    ├── hpa.yaml
    └── _helpers.tpl
```

### Installation and Usage

1. Install Helm (if not already installed):
   ```bash
   curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
   ```

2. Add dependencies:
   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   helm repo update
   ```

3. Deploy to different environments:
   ```bash
   # Development
   helm install webapp-dev ./helm/webapp -f helm/webapp/values-dev.yaml -n development

   # Staging
   helm install webapp-staging ./helm/webapp -f helm/webapp/values-staging.yaml -n staging

   # Production
   helm install webapp-prod ./helm/webapp -f helm/webapp/values-prod.yaml -n production
   ```

### Configuration Options

Key configurations available in values files:

```yaml
webapp:
  replicaCount: 1
  image:
    repository: nanajanashia/k8s-demo-app
    tag: v1.0
    pullPolicy: IfNotPresent
  
  resources:
    requests:
      cpu: 250m
      memory: 256Mi
    limits:
      cpu: 500m
      memory: 512Mi

  service:
    type: NodePort
    port: 3000
    nodePort: 30100

mongodb:
  enabled: true
  auth:
    rootPassword: mongopassword
    username: mongouser
    password: mongopassword
```

### Upgrading and Rolling Back

- Upgrade a release:
  ```bash
  helm upgrade webapp-dev ./helm/webapp -f helm/webapp/values-dev.yaml -n development
  ```

- Roll back to a previous release:
  ```bash
  # List release history
  helm history webapp-dev -n development

  # Roll back to a specific revision
  helm rollback webapp-dev 1 -n development
  ```

### Helm Commands Reference

```bash
# Validate chart
helm lint ./helm/webapp

# See what will be installed
helm template webapp-dev ./helm/webapp -f values-dev.yaml

# List installed releases
helm list -n development

# Uninstall a release
helm uninstall webapp-dev -n development
```

---

## GitOps Implementation

This project uses ArgoCD for GitOps-based deployments. The setup includes:

1. Install ArgoCD:
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

2. Access ArgoCD UI:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```
   Get the initial admin password:
   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
   ```

3. Deploy Applications:
   ```bash
   kubectl apply -f argocd/projects/webapp.yaml
   ```

### Environment Management

The project supports three environments:
- **Development**: Automatic sync with pruning enabled
- **Staging**: Automatic sync with pruning enabled
- **Production**: Manual sync for safety

Each environment is configured in:
- `helm/webapp/values-{env}.yaml`: Environment-specific configurations
- `argocd/projects/webapp.yaml`: ArgoCD application definitions

### GitOps Workflow

1. Make changes to the Helm chart or values
2. Commit and push to the repository
3. ArgoCD automatically detects changes
4. Changes are applied based on environment sync policies

---

## Contributing

Contributions are welcome! If you'd like to improve this project, please follow these steps:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m 'Add some feature'`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a Pull Request.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
