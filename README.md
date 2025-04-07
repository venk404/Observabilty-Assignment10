# SRE Project - Setting Up an Observability Stack

## Problem Statement
Implementing a comprehensive observability stack to monitor Kubernetes clusters and applications using Prometheus, Loki, Grafana, and various exporters.

## Prerequisites
- Docker
- Kind
- Kubectl
- Helm
- Kubernetes cluster

## Installation Guide

### 1. Clone the Repository
```bash
git clone https://github.com/venk404/SRE--RestAPI.git
cd "Assignment 10"
```

### 2. Create Observability Namespace
```bash
kubectl create namespace observability
```

### 3. Install kube-prometheus-stack
```bash
helm install prometheus ./kube-prometheus-stack/ --values ./prometheus.yaml -n observability
```

### 4. Install Blackbox Exporter
```bash
helm install blackbox-exporter ./prometheus-blackbox-exporter/ --values ./blackbox-exportor.yaml -n observability
```

### 5. Install Postgres Exporter
```bash
helm install postgres-exportor ./prometheus-postgres-exporter/ --values ./postgres-exportor.yaml -n observability
```

Note: The installation includes external secret templates that create secrets used by the Postgres exporter.

### 6. Deploy PLG Stack (Promtail, Loki, Grafana)
```bash
helm upgrade --install loki ./loki-stack/ --values './Loki values.yaml' -n observability
```

### 7. Port Forward Services for Local Access

#### Access Prometheus:
```bash
kubectl port-forward svc/prometheus-kube-prometheus-prometheus 9090:9090 -n observability
```
Prometheus URL: http://localhost:9090

#### Access Grafana:
```bash
kubectl port-forward svc/loki-grafana 3000:80 -n observability
```
Grafana URL: http://localhost:3000
Default credentials:
- Username: admin
- Password: (Retrieve with command below)
```bash
kubectl get secret loki-grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```

### 8. Access the REST API
```
http://127.0.0.1:30007/docs
```

## Conclusion
This stack provides complete visibility into system performance and health across your Kubernetes environment.

![Deploy Observailty Stack](https://img.notionusercontent.com/s3/prod-files-secure%2F9ce3a364-243d-4bf8-803e-331bbc517340%2F9c32853f-89c0-424e-8b04-f7bc12c6d12b%2Fobs-k8s-deployment.drawio.png/size/?exp=1744145476&sig=SEOHMNuS8K2hUIgvbIiFNAiC4lFNISozqEwY0_MV5YQ&id=50b472ac-1b05-4f82-830f-e3bf1efc7719&table=block)