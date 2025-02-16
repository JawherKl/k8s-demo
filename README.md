# Kubernetes Demo Project

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
│   ├── service.yaml         # Service configuration
│   ├── configmap.yaml       # ConfigMap for environment variables
│   └── ...                 # Other Kubernetes resources
├── src/                     # Application source code (if applicable)
├── Dockerfile               # Dockerfile for building the application image
└── README.md                # This file
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
