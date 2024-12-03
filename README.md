## CI/CD Pipeline for Frontend and Backend with Minikube and Jenkins and Helm

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

## Setup Instructions

### 1. EC2 Setup (Jenkins & Minikube)

Since both Jenkins and Minikube will run on the same EC2 instance, follow these steps to set up your environment:

#### 1.1. Install Jenkins

1. **Update system and install Java:**

   ```bash
   sudo apt update
   sudo apt install openjdk-11-jdk
   ```
   
2. **Install Jenkins:**

   ```bash
   wget -q -O - https://pkg.jenkins.io/jenkins.io.key | sudo apt-key add -
   sudo sh -c 'echo deb http://pkg.jenkins.io/debian/ $(lsb_release -cs) main > /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   sudo apt install jenkins
   ```

3. **Start Jenkins:**

   ```bash
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   ```
   ![Alt Text](/images/JK-8.JPG)
4. **Access Jenkins UI:**

   - **Navigate to Jenkins UI:**
     - Open your browser and go to:  
    `http://<your-ec2-public-ip>:8080`

   - **Unlock Jenkins:**
     - Retrieve the initial setup password by running the following command:  
       ```bash
       cat /var/lib/jenkins/secrets/initialAdminPassword
       ```
  - Copy the password and paste it into the Jenkins UI to unlock Jenkins and complete the setup.

   ![Alt Text](/images/JK-1.JPG)

   ![Alt Text](/images/JK-2.JPG)

#### 1.2. Install Minikube

1. **Install dependencies::**

   ```bash
   sudo apt-get install -y curl apt-transport-https virtualbox virtualbox-ext-pack
   ```
   
2. **Install Minikube:**

   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
   ```

3. **Start Minikube:**

   ```bash
   minikube start
   ```

4. **Verify Minikube installation:**

   ```bash
   kubectl get nodes
   ```

#### 1.3. Install Helm

1. **Download Helm:**

   ```bash
   curl https://get.helm.sh/helm-v3.7.1-linux-amd64.tar.gz -o helm-v3.7.1-linux-amd64.tar.gz
   tar -zxvf helm-v3.7.1-linux-amd64.tar.gz
   sudo mv linux-amd64/helm /usr/local/bin/helm
   ```
2.  **Verify Helm installation:**

    ```bash
    helm version
    ```

   ![Alt Text](/images/cap-helm-installation.JPG)

### 2. Docker Setup 

Make sure Docker is installed on the EC2 instance where Jenkins and Minikube are running.

![Alt Text](/images/cap-docker-login-push.JPG)


![Alt Text](/images/cap-docker-push-backend.JPG)

#### 2.1. Install Docker

```bash
sudo apt-get update
sudo apt-get install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```
 ![Alt Text](/images/cap-dockerhub.JPG)
### 3. Configure Jenkins Pipeline

1. **Create a Jenkins job:**
   - In Jenkins, create a new pipeline job and point it to the GitHub repository containing the `Jenkinsfile`.
   - Make sure Jenkins is configured with the necessary credentials (e.g., Docker Hub credentials).

    ![Alt Text](/images/JK-7.JPG)

   
3. **Configure Jenkinsfile:**
   The `Jenkinsfile` in the repository automates the build, push, and deploy processes. Here's a breakdown of its steps:
   - **Checkout:** The pipeline checks out the code from the GitHub repository.
   - **Build Frontend Docker Image:** The frontend Docker image is built using the `Dockerfile` in the `frontend/` directory.
   - **Build Backend Docker Image:** Similarly, the backend Docker image is built using the `Dockerfile` in the `backend/` directory.
   - **Push Docker Images:** Both the frontend and backend images are pushed to Docker Hub.
   - **Deploy with Helm:** The Helm charts in the `frontend/helm` and `backend/helm` directories are used to deploy the applications to the Minikube Kubernetes cluster.

### 4. Running the Pipeline

Whenever changes are pushed to GitHub, the Jenkins pipeline is triggered, which follows this flow:

- **Code changes:** Push changes to GitHub.
- **Jenkins triggers:** Jenkins checks out the latest code.
- **Build Docker images:** Jenkins builds Docker images for the frontend and backend.
- **Push Docker images:** Jenkins pushes the images to Docker Hub.
- **Deploy to Minikube:** Helm is used to deploy the images to the Minikube Kubernetes cluster.

   ![Alt Text](/images/cap-backend-pod-running.JPG)

   ![Alt Text](/images/cap-frontend-pod-running.JPG)

### 5. Conclusion

With the above setup, you’ve created an automated CI/CD pipeline that builds and deploys both the frontend and backend applications. Jenkins runs the pipeline, and Minikube ensures that your deployments are made to a local Kubernetes cluster. Docker Hub stores the images for both applications, while Helm charts are used for seamless deployment on Kubernetes.

![Alt Text](/images/cap-website.JPG)

### 6. Contributing

I welcome contributions! To contribute:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Commit your changes with clear messages.
4. Submit a pull request for review.

Make sure to follow the code style guidelines and include proper documentation for any new features.


### 7. Contact

For any queries, feel free to contact me:

- **Email:** adityavakharia@gmail.com
- **GitHub:** [Aditya-rgb](https://github.com/Aditya-rgb/Container-Orchestration)

You can also open an issue in the repository for questions or suggestions.


