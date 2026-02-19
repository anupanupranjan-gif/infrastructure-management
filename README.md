
## **2️⃣ infrastructure-management/README.md**
## Project Overview

The Infrastructure Management repository provides Kubernetes manifests and observability configuration for deploying and monitoring the Commerce Search Platform.  
It includes deployments, services, ServiceMonitors, and dashboards for Prometheus and Grafana, enabling full metrics, logging, and visualization of both the Spring Boot application and Elasticsearch cluster.

This repo separates infrastructure concerns from application code, allowing DevOps and platform teams to manage deployment, scaling, and monitoring independently.


# Infrastructure Management

This repository contains Kubernetes manifests and monitoring configuration for the Commerce Search Platform.  
It manages deployments, services, ServiceMonitors, Prometheus, and Grafana dashboards.

## Project Structure
.
├── k8s
│ ├── namespace.yaml # Kubernetes namespaces
│ ├── elasticsearch # Elasticsearch manifests
│ │ ├── elasticsearch-deployment.yml
│ │ ├── elasticsearch-service.yml
│ │ └── exporter-servicemonitor.yml
│ └── springboot # Spring Boot manifests
│ ├── springboot-deployment.yml
│ ├── springboot-service.yml
│ └── servicemonitor.yaml
├── prometheus
│ └── custom-scrape-config.yml # Custom Prometheus scrape config
├── grafana
│ └── dashboards
│ ├── elasticsearch-dashboard.json
│ └── springboot-dashboard.json
└── README.md


## Getting Started

### Prerequisites
- Kubernetes cluster (tested with `kind` on aarch64 Fedora VM)
- kubectl CLI
- Docker (to build Spring Boot image for kind cluster)
- Prometheus & Grafana installed via kube-prometheus stack

### Deploy Application

```bash
# Apply namespaces
kubectl apply -f k8s/namespace.yaml

# Deploy Elasticsearch
kubectl apply -f k8s/elasticsearch/

# Deploy Spring Boot app
kubectl apply -f k8s/springboot/

# Restart deployments if needed
kubectl rollout restart deployment advanced-search-app -n commerce

Observability

Prometheus scrapes metrics from Spring Boot and Elasticsearch via ServiceMonitors

Grafana dashboards included for visualization

springboot-dashboard.json

elasticsearch-dashboard.json

Notes

Spring Boot pods use the ELASTICSEARCH_URL environment variable to connect to Elasticsearch

Ensure Docker image advanced-search-app:latest is loaded into the kind cluster:

kind load docker-image advanced-search-app:latest

