# ShvikiFitness GitOps 🏋️‍♀️

An Argo CD App of Apps for deploying the ShvikiFitness application and its related infrastructure components. This project provides a GitOps-driven approach to manage the deployment of the ShvikiFitness application (Flask + MySQL), monitoring stack (kube-prometheus-stack), cluster autoscaler, and external secrets operator. It simplifies the deployment and management of the entire ShvikiFitness environment by orchestrating the deployment of other Argo CD Applications.

## 🚀 Features

- **App of Apps**: Manages the deployment of multiple Argo CD Applications as a single unit.
- **GitOps Driven**: Uses Git as the single source of truth for application deployments.
- **Automated Synchronization**: Automatically synchronizes application state with the Git repository.
- **Infrastructure as Code**: Defines infrastructure components (monitoring, autoscaling, secrets) as code.
- **Node Placement Rules**: Configures node selectors and tolerations for workload deployments.
- **Centralized Configuration**: Manages all application configurations in a single `values.yaml` file.

## 🛠️ Tech Stack

| Category      | Technology                      | Description                                                                 |
|---------------|---------------------------------|-----------------------------------------------------------------------------|
| **Orchestration** | Argo CD                         | Declarative, GitOps continuous delivery tool for Kubernetes.                |
| **Containerization**| Docker                          | Used for containerizing the ShvikiFitness application.                    |
| **Configuration Management** | Helm                          | Package manager for Kubernetes, used to deploy the applications.        |
| **Monitoring**    | kube-prometheus-stack           | Prometheus-based monitoring solution for Kubernetes.                      |
| **Autoscaling**   | Kubernetes Cluster Autoscaler   | Automatically adjusts the size of the Kubernetes cluster.                 |
| **Secrets Management**| External Secrets Operator     | Integrates external secret management systems with Kubernetes.            |
| **Backend**       | MySQL                           | Database for the ShvikiFitness application.                               |
| **Frontend**      | Flask (Python)                  | Web framework for the ShvikiFitness application.                           |
| **Infrastructure**| Kubernetes                      | Container orchestration platform.                                         |
| **Cloud Provider**| AWS                             | Cloud provider for the Kubernetes cluster (specified in `values.yaml`).    |
| **Other**         | Git                             | Version control system for managing application configurations.           |

## 📦 Getting Started

### Prerequisites

- [Helm](https://helm.sh/docs/intro/install/) installed
- [Argo CD](https://argo-cd.readthedocs.io/en/stable/getting_started/) installed and configured in your Kubernetes cluster
- [kubectl](https://kubernetes.io/docs/tasks/tools/) configured to connect to your Kubernetes cluster
- Access to a Kubernetes cluster (e.g., Minikube, Kind, or a cloud-based cluster)

### Installation

1.  Clone the repository:

    ```bash
    git clone <repository_url>
    cd shviki-fitness-gitops
    ```

2.  Customize the `values.yaml` file:

    Modify the `helm/values.yaml` file to match your environment.  Pay close attention to:
    - `global.clusterName`: Set to your Kubernetes cluster name.
    - `global.awsRegion`: Set to your AWS region (if applicable).
    - `apps.shvikiFitness.repoURL`: Set to the repository URL of your ShvikiFitness application.
    - `apps.monitoring`, `apps.clusterAutoscaler`, `apps.externalSecrets`: Verify the chart versions and repository URLs.

3.  Deploy the Helm chart:

    ```bash
    helm install shviki-fitness ./helm -n argocd --create-namespace
    ```
    (Assumes Argo CD is installed in the `argocd` namespace. Adjust accordingly.)

### Running Locally

After deploying the Helm chart, Argo CD will automatically start deploying the applications defined in the `values.yaml` file. You can monitor the progress in the Argo CD UI.

1.  Access the Argo CD UI:

    ```bash
    kubectl port-forward svc/argo-cd-server -n argocd 8080:443
    ```

    Then, open your browser and navigate to `https://localhost:8080`.

2.  Monitor the application deployments:

    In the Argo CD UI, you should see the `shviki-fitness` application and its dependencies being deployed.

## 💻 Usage

Once the applications are deployed, you can access the ShvikiFitness application and the monitoring stack.

- **ShvikiFitness Application**: Access the application through the service exposed by the ShvikiFitness deployment.
- **Monitoring Stack**: Access the Prometheus and Grafana UIs through the services exposed by the `kube-prometheus-stack` deployment.

## 📂 Project Structure

```
shviki-fitness-gitops/                # GitOps repo managing deployment state via Argo CD
├── helm                              # Helm chart controlling GitOps-managed resources
│   ├── Chart.yaml                    # Metadata about the chart (name, version, dependencies)
│   ├── templates                     # Kubernetes resources rendered and applied by Helm
│   │   ├── cluster-autoscaler.yaml    # Config for Cluster Autoscaler to scale worker nodes
│   │   ├── external-secrets.yaml      # Fetch secrets from AWS Secret Manager / ESO integration
│   │   ├── monitoring.yaml            # Metrics, Grafana/Prometheus dashboards, alerts, etc.
│   │   └── shviki-fitness.yaml        # Argo CD Application that deploys the main app from Git
│   └── values.yaml                   # Default values for templating and environment overrides
└── README.md                         # Documentation for GitOps installation and usage
```

