# Container-Orchestration

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
