# Flask Expense Tracker - DevOps CI/CD Project

A complete DevOps implementation of a containerized expense tracking application featuring automated CI/CD pipeline with Jenkins, Docker, Kubernetes, and AWS ECR integration.

## 🏗️ Architecture Overview

![Architecture Diagram](readme/arc.jpg)

### Technology Stack
- **Frontend/Backend**: Flask (Python)
- **Database**: MongoDB with persistent storage
- **Containerization**: Docker
- **Container Registry**: AWS ECR
- **Orchestration**: Kubernetes
- **CI/CD**: Jenkins with GitOps workflow
- **Version Control**: GitHub with webhooks
- **Infrastructure**: Kubernetes cluster with persistent volumes

## 🚀 CI/CD Pipeline Workflow

### Pipeline Stages
1. **Code Clone** - Pulls latest code from GitHub repository
2. **Build Image** - Creates Docker image with Jenkins build number tag
3. **Push to ECR** - Uploads image to AWS Elastic Container Registry
4. **Image Cleanup** - Removes local Docker image to save space
5. **Tag Update** - Updates Kubernetes deployment manifest with new image tag
6. **Deploy** - Applies updated manifests to Kubernetes cluster

### GitOps Workflow
- Code push triggers Jenkins pipeline via GitHub webhook
- Pipeline builds and pushes new image to ECR
- Kubernetes manifest updated with new image tag
- Changes committed back to repository
- Kubernetes deployment automatically updated

## 📋 Prerequisites

- Jenkins server with Docker and kubectl installed
- AWS ECR repository configured
- Kubernetes cluster (local or cloud)
- GitHub repository with webhook configuration
- Jenkins credentials for GitHub and AWS

## 🛠️ Setup Instructions

### 1. Jenkins Configuration
```bash
# Install required plugins
- Docker Pipeline
- Kubernetes CLI
- GitHub Integration
```

### 2. Credentials Setup
- **GitHub**: Username/password or token for repository access
- **AWS**: ECR access credentials
- **Kubernetes**: Cluster access configuration

### 3. Pipeline Job Creation
1. Create new Pipeline job in Jenkins
2. Point to this repository's Jenkinsfile
3. Configure GitHub webhook trigger
4. Set environment variables

### 4. Deployment
```bash
# Manual deployment (if needed)
kubectl apply -f k8s/

# Check deployment status
kubectl get pods -n et-ns
kubectl get services -n et-ns
```

## 🔧 Environment Configuration

### Jenkins Environment Variables
- `VERSION`: Build number from Jenkins (`${BUILD_NUMBER}`)
- `IMAGE`: ECR repository URL (`188381035573.dkr.ecr.ap-south-1.amazonaws.com/expensetracker`)

### Kubernetes Resources
- **Namespace**: `et-ns`
- **MongoDB**: StatefulSet with 1Gi persistent storage
- **Flask App**: Deployment with 2 replicas
- **Services**: ClusterIP for MongoDB, NodePort for Flask app
- **Storage**: PersistentVolumeClaim for MongoDB data

## 🌐 Access Points

- **Application URL**: `http://<cluster-ip>:30190`
- **MongoDB**: Internal service at `mongo-service:27017`

## 📊 Monitoring & Troubleshooting

```bash
# Check application status
kubectl get all -n et-ns

# View application logs
kubectl logs -f deployment/app-deployment -n et-ns

# Check MongoDB logs
kubectl logs -f statefulset/mongo-statefulset -n et-ns

# Describe resources for troubleshooting
kubectl describe pod <pod-name> -n et-ns
```

## 🔄 GitOps Process

1. Developer pushes code to GitHub
2. GitHub webhook triggers Jenkins pipeline
3. Jenkins builds Docker image with build number
4. Image pushed to AWS ECR
5. Kubernetes manifest updated with new image tag
6. Changes committed back to repository
7. Kubernetes pulls new image and updates deployment

## 📁 Project Structure

```
expense-tracker-CICD/
├── Jenkins/
│   └── jenkinsfile          # CI/CD pipeline definition
├── k8s/
│   ├── app-deployment.yml   # Flask app deployment
│   ├── app-service.yml      # Flask app service
│   ├── configmap.yml        # Configuration data
│   ├── mongo-service.yml    # MongoDB service
│   ├── mongo-statfulset.yml # MongoDB StatefulSet
│   ├── namespace.yml        # Kubernetes namespace
│   └── pvc.yml             # Persistent volume claim
├── readme/
│   └── architecture-diagram.png
├── app.py                   # Flask application
├── Dockerfile              # Container image definition
└── README.md               # Project documentation
```

## 🚦 Pipeline Status

The pipeline automatically triggers on every push to the main branch and provides:
- Automated testing and building
- Container image versioning
- Zero-downtime deployments
- Rollback capabilities through Git history

---

**Note**: Ensure all prerequisites are met and credentials are properly configured before running the pipeline.