# Flask Expense Tracker CI/CD

A containerized expense tracking application with automated CI/CD pipeline using Jenkins, Docker, and Kubernetes.

## Architecture

- **Application**: Flask web app with MongoDB
- **CI/CD**: Jenkins pipeline with GitOps workflow
- **Container Registry**: AWS ECR
- **Orchestration**: Kubernetes
- **Storage**: Persistent volumes for MongoDB

## Pipeline Stages

1. **Code Clone** - Pulls latest code from GitHub
2. **Build Image** - Creates Docker image with build number tag
3. **Push to ECR** - Uploads image to AWS Elastic Container Registry
4. **Image Cleanup** - Removes local Docker image
5. **Tag Update** - Updates Kubernetes manifest with new image tag
6. **Deploy** - Applies updated manifests to Kubernetes cluster

## Quick Start

### Prerequisites
- Jenkins with Docker and kubectl
- AWS ECR repository
- Kubernetes cluster
- GitHub repository with webhook

### Setup
1. Configure Jenkins credentials for GitHub and AWS
2. Create Jenkins pipeline job pointing to this repository
3. Set up GitHub webhook to trigger builds
4. Deploy: Pipeline runs automatically on code push

### Access
- Application: `http://<cluster-ip>:30190`
- Monitor: `kubectl get pods -n et-ns`

## Environment Variables

- `VERSION`: Build number from Jenkins
- `IMAGE`: ECR repository URL

## Kubernetes Resources

- Namespace: `et-ns`
- MongoDB StatefulSet with persistent storage
- Flask app deployment (2 replicas)
- Services and ConfigMaps
- PersistentVolumeClaim (1Gi)