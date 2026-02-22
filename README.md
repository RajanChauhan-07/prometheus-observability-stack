# 🔥 Prometheus Observability Stack on AWS

A complete production-grade monitoring stack deployed on AWS EKS using Terraform and Helm.

## 🏗️ Architecture

- **Cloud Provider:** AWS (EKS)
- **Cluster Provisioning:** Terraform
- **Monitoring Tools:** Prometheus + Grafana + AlertManager (via Helm)
- **Load Testing:** BusyBox load generator

## 📦 Stack Components

| Component | Purpose |
|-----------|---------|
| Prometheus | Metrics collection & storage |
| Grafana | Visualization & dashboards |
| AlertManager | Alert routing & management |
| Node Exporter | Node-level metrics |
| Kube State Metrics | Kubernetes object metrics |

## 🗂️ Project Structure
```
prometheus-stack/
├── terraform/
│   ├── main.tf         # EKS cluster & VPC
│   ├── variables.tf    # Configuration variables
│   ├── outputs.tf      # Cluster outputs
│   └── versions.tf     # Provider versions
├── helm/
│   ├── values.yaml     # Helm chart configuration
│   └── alert-rules.yaml # Custom alert rules
└── README.md
```

## 🚀 Deployment Steps

### Prerequisites
- AWS CLI
- Terraform v1.5+
- kubectl
- Helm v3+

### 1. Configure AWS
```bash
aws configure
```

### 2. Deploy EKS Cluster
```bash
cd terraform
terraform init
terraform plan
terraform apply
```

### 3. Connect kubectl
```bash
aws eks update-kubeconfig --region us-east-1 --name prometheus-stack-cluster
```

### 4. Deploy Monitoring Stack
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
kubectl create namespace monitoring
helm install prometheus-stack prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --values helm/values.yaml
```

### 5. Apply Alert Rules
```bash
kubectl apply -f helm/alert-rules.yaml
```

### 6. Access Grafana
```bash
kubectl get svc -n monitoring
# Open EXTERNAL-IP in browser
# Username: admin
# Password: prometheus123
```

## 📊 Dashboards

Import Grafana dashboard ID: `15760` for Kubernetes monitoring.

## 🚨 Alert Rules

- **HighCPUUsage** — Fires when CPU > 50% for 1 minute
- **HighMemoryUsage** — Fires when Memory > 50% for 1 minute  
- **PodCrashLooping** — Fires when a pod restarts repeatedly

## 🧹 Cleanup
```bash
cd terraform
terraform destroy
```

## 📸 Screenshots

![Grafana Dashboard](![WhatsApp Image 2026-02-22 at 13 01 42](https://github.com/user-attachments/assets/f8a5633c-287b-41e0-8d9c-e1993d98918f)
)
![AlertManager](![WhatsApp Image 2026-02-22 at 13 13 40](https://github.com/user-attachments/assets/cc27a2f0-e75a-403a-a28e-27053865897a)
)
