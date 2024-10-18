# CI/CD Pipeline for Frontend and Backend Deployment using Google Cloud Build, Helm, and GKE

This documentation outlines the steps to create an efficient CI/CD pipeline using Google Cloud Build and Helm for automatic deployments on Google Kubernetes Engine (GKE). The process is designed for quick feedback, automation, and scalability.

[CI/CD Pipeline]![image](https://github.com/user-attachments/assets/eb692fb1-e723-454e-8ff4-2a8e641fc9ed)


## Overview

- **Tools Used**: Google Cloud Build, GKE, Helm, Secret Manager, GCR (Google Container Registry)
- **Pipeline Purpose**: Automate the building, testing, and deployment of frontend and backend microservices.
- **Source**: Integrated with GitHub for automatic triggers on commit.

## Steps to Create the CI/CD Pipeline

### 1. Set Up Build Triggers
- **Trigger Creation**: Start by creating build triggers in Google Cloud Build.
- **GitHub Integration**: Integrate your GitHub repository with Google Cloud Build for automatic triggers on commit or pull request.
- **Select Zone and Cluster**: Configure the zone (e.g., `asia-south1-a`) and cluster (e.g., `gke-cluster-1`) where your GKE resources are located.
- **Branch and Repo Selection**: Add the specific repository and branch (e.g., `main`, `dev`) to monitor for changes.
- **Commit Approval Process**: Enable an approval step to manually review changes before deployment, ensuring higher quality control.

### 2. Authenticate Service Account for Access
- **Service Account Creation**: Create and assign a service account in GCP with the necessary permissions (Cloud Build, GKE, Secret Manager, and Cloud Container Registry).
- **Grant Access**: The service account should have roles to authenticate with GKE, push images to GCR, and access Secret Manager for sensitive information.

### 3. Store Environment Variables and Secrets
- **Secret Manager Setup**: Use Google Cloud Secret Manager to securely store environment variables and secrets (such as database credentials, API keys, etc.).
- **Example Secret**: Store `.env` configurations for backend and frontend services in Secret Manager.

### 4. Create Google Container Registry (GCR)
- **Container Registry Setup**: Set up a GCR bucket to store Docker images. This will store the built images of your frontend and backend applications.
- **Private Image Repo**: Ensure the repository is private and access-controlled by the service account for security purposes.

## CI/CD Pipeline Steps

### Step 1: Retrieve Secrets and Pass Environment Variables
- Use Secret Manager to retrieve the secrets for your applications and pass them to the Docker build process.
- Secrets like `.env` files are retrieved securely during the build process.

### Step 2: Build Docker Images for Frontend and Backend Services
- **Docker Build**: Use the Dockerfiles in your frontend and backend services to build images for each microservice.

### Step 3: Push Docker Images to Google Container Registry (GCR)
- **Push to GCR**: After successfully building the images, push them to GCR.

### Step 4: Authenticate with GKE and Set Up Kubernetes Context
- **GKE Authentication**: Use Google Cloud SDK to authenticate with your GKE cluster and set up the Kubernetes context for Helm deployment.

### Step 5: Install Helm for Kubernetes Package Management
- **Install Helm**: Install Helm to manage the deployment of Kubernetes applications using Helm charts.

### Step 6: Update Image Tags in `values.yaml`
- **Update Docker Image Tags**: Modify the `values.yaml` file in the Helm charts to use the newly built and tagged Docker images.

### Step 7: Deploy Helm Chart to GKE
- **Helm Deployment**: Use Helm to deploy or upgrade your applications on GKE.

### Step 8: Add Substitutions for Parameterized Builds
- Add custom substitutions to handle environment-specific parameters dynamically. These could include project ID, region, image tags, and environment details.

## Conclusion

This CI/CD pipeline automates the building, testing, and deployment of your frontend and backend microservices on GKE using Helm and Google Cloud Build. It ensures quick feedback and reliable deployment through automatic GitHub integration, secret management, and containerization best practices. The substitution feature allows easy parameterization for different environments, improving scalability and flexibility.
