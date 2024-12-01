# Container-Orchestration

# CI/CD Pipeline for Frontend and Backend with Minikube and Jenkins

This repository is designed to automate the process of building Docker images for both frontend and backend codebases, pushing them to Docker Hub, and deploying the application to a Kubernetes cluster running on Minikube. The CI/CD pipeline is managed using Jenkins, which is configured to monitor changes in the GitHub repository, build Docker images, push them to Docker Hub, and deploy them to Minikube.

## Project Overview

The goal of this project is to implement a streamlined Continuous Integration and Continuous Deployment (CI/CD) pipeline to handle the following tasks:

- Build Docker images for both frontend and backend applications
- Push the Docker images to Docker Hub
- Deploy the applications to a Kubernetes cluster running locally on Minikube
- Automate the entire process using Jenkins for monitoring and deployment

## Features

- **Automated Docker Image Build**: Automatically builds Docker images for both frontend and backend whenever there are updates in the repository.
- **Docker Hub Integration**: Pushes the built Docker images to Docker Hub to make them accessible for deployment.
- **Minikube Integration**: Uses Minikube to deploy the Docker images to a local Kubernetes cluster.
- **Jenkins Automation**: Automates the entire process with Jenkins, including monitoring repository changes, building images, and deploying them.

# Technologies Used

- **Frontend & Backend**: The project consists of a frontend and backend application, each containerized with Docker. Each component has its own Dockerfile and Helm chart to handle deployment to the Kubernetes cluster.

- **Docker**: Used to containerize both frontend and backend applications. Docker ensures that both components are portable and can run consistently across different environments.

- **Jenkins**: Jenkins automates the CI/CD pipeline, continuously monitoring the GitHub repository for any changes. When a change is detected, Jenkins automatically builds the Docker images for both the frontend and backend, pushes them to Docker Hub, and deploys them to the Kubernetes cluster on Minikube.

- **Helm**: Helm is a Kubernetes package manager that simplifies the deployment and management of applications on Kubernetes. It is used to create Helm charts for the frontend and backend, making deployment to the cluster more efficient and repeatable.

- **Minikube**: Minikube is a tool for running a local Kubernetes cluster. In this project, it is deployed on an EC2 instance to simulate a production-like environment for development and testing. Minikube provides the same Kubernetes features as a full-scale production cluster but runs locally for ease of use.

# Repository Structure

```bash
/root (or your project root)
│
├── frontend/                         # Directory for the frontend code
│   ├── Dockerfile                    # Dockerfile for the frontend application
│   └── helm/                         # Directory containing the Helm chart for frontend
│       └── Chart.yaml                # Helm chart metadata
│       └── values.yaml               # Default values for the Helm chart
│       └── templates/                # Helm templates for Kubernetes resources (e.g., Deployment, Service)
│
├── backend/                          # Directory for the backend code
│   ├── Dockerfile                    # Dockerfile for the backend application
│   └── helm/                         # Directory containing the Helm chart for backend
│       └── Chart.yaml                # Helm chart metadata
│       └── values.yaml               # Default values for the Helm chart
│       └── templates/                # Helm templates for Kubernetes resources (e.g., Deployment, Service)
│
└── Jenkinsfile                       # The pipeline script (Groovy) for Jenkins
```

## Prerequisites

Before setting up the project, make sure the following are installed:

- Docker
- Minikube
- Kubernetes (kubectl)
- Jenkins
- GitHub repository containing frontend and backend code

## Setup and Installation

### Setup Instructions
1. EC2 Setup (Jenkins & Minikube)
Since both Jenkins and Minikube will run on the same EC2 instance, follow these steps to set up your environment:

1.1. Install Jenkins
Update system and install Java:
```bash
sudo apt update
sudo apt install openjdk-11-jdk
```

Install Jenkins:
```bash

wget -q -O - https://pkg.jenkins.io/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian/ $(lsb_release -cs) main > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins

Start Jenkins:
```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
