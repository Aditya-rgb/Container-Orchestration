pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials' // Replace with your Docker Hub credentials ID
        DOCKER_HUB_REPO_FRONTEND = 'your-dockerhub-repo/frontend'
        DOCKER_HUB_REPO_BACKEND = 'your-dockerhub-repo/backend'
        MINIKUBE_KUBECONFIG_PATH = '/home/ubuntu/.kube/config' // Path to Minikube kubeconfig on the EC2 instance
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    script {
                        sh 'docker build -t ${DOCKER_HUB_REPO_FRONTEND}:latest .'
                    }
                }
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    script {
                        sh 'docker build -t ${DOCKER_HUB_REPO_BACKEND}:latest .'
                    }
                }
            }
        }

        stage('Push Frontend Docker Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        sh 'docker push ${DOCKER_HUB_REPO_FRONTEND}:latest'
                    }
                }
            }
        }

        stage('Push Backend Docker Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        sh 'docker push ${DOCKER_HUB_REPO_BACKEND}:latest'
                    }
                }
            }
        }

        stage('Deploy Frontend with Helm') {
            steps {
                script {
                    sh "kubectl --kubeconfig=${MINIKUBE_KUBECONFIG_PATH} apply -f frontend/helm"
                }
            }
        }

        stage('Deploy Backend with Helm') {
            steps {
                script {
                    sh "kubectl --kubeconfig=${MINIKUBE_KUBECONFIG_PATH} apply -f backend/helm"
                }
            }
        }
    }
}
