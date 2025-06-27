
# DevOp-GitOps

A GitOps-driven Kubernetes deployment repository powered by Jenkins, Docker, Ansible, and ArgoCD.

## 🔧 Overview

This repository automates the deployment of a containerized application (`posts_proj`) to a Kubernetes cluster. It integrates with Jenkins to update Kubernetes manifests, build Docker images, and push changes, while ArgoCD continuously syncs the updated manifests to the cluster.

## 🧰 Tech Stack

- 🐳 **Docker** — Build and tag application images
- ⚙️ **Jenkins** — CI/CD automation
- 📦 **Ansible** — Update Kubernetes YAML manifests
- ☸️ **Kubernetes** — Container orchestration
- 🚀 **ArgoCD** — GitOps delivery
- 📁 **GitHub** — Source of truth for manifests

## 🚀 GitOps Flow

1. Jenkins builds the Docker image and tags it with the Jenkins build number.
2. Ansible updates the `image` tag in `k8s/app.yml`.
3. Jenkins commits and pushes the updated manifest to the GitHub repo.
4. ArgoCD detects the change and automatically deploys the updated app to the Kubernetes cluster.

## 📁 Directory Structure

```

.
├── Jenkinsfile          # CI/CD pipeline logic
├── update-tag.yml       # Ansible playbook to update image tag
└── k8s/
└── app.yml          # Kubernetes manifest for the app

````

## ⚙️ Jenkins Environment Variables

| Variable      | Description                          |
|---------------|--------------------------------------|
| `DOCKER_IMAGE`| Docker Hub image name                |
| `IMAGE_TAG`   | Tag applied to Docker image and YAML |

## 🔐 Secrets & Credentials

Jenkins should have the following credentials set up:

- **GitHub** – Git username/password or SSH key (for push access)
- **Docker Hub** – Username and password
- **ArgoCD** – No interaction needed, it auto-syncs from Git

## 📦 Example Image Tag Update

Before:
```yaml
image: tareqmohamed/posts_proj:18
````

After Jenkins build #19:

```yaml
image: tareqmohamed/posts_proj:19
```

