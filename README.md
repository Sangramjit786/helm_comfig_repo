# Helm Configurations for SpringBoot Banking Services

This repository contains **Helm charts** and configuration templates used to deploy the [springboot-banking-services](https://github.com/Sangramjit786/springboot-banking-services.git) project into Kubernetes clusters.  
It provides Helm charts for each microservice, environment-based configurations (Dev, QA, Prod), and observability tools like Prometheus and Grafana. 

## Table of Contents

1. [Project Overview](#project-overview)
2. [Prerequisites](#prerequisites)
3. [Architecture](#architecture)
4. [Helm Charts](#helm-charts)
5. [Installation](#installation)
6. [Environment Setup](#environment-setup)
7. [Monitoring & Observability](#monitoring--observability)
8. [Security](#security)
9. [Troubleshooting](#troubleshooting)

## Project Overview

The application consists of the following microservices:
- **Accounts Service**: Manages customer accounts
- **Cards Service**: Handles credit card operations
- **Loans Service**: Manages loan products
- **Config Server**: Centralized configuration management
- **Eureka Server**: Service discovery
- **Gateway Server**: API Gateway using Spring Cloud Gateway

## Prerequisites

- Kubernetes cluster (Minikube, EKS, AKS, or GKE)
- Helm 3.x
- kubectl
- Docker
- Java 21+
- Maven 3.6+

## Architecture

The application follows a microservices architecture with:
- Service Discovery (Eureka)
- API Gateway (Spring Cloud Gateway)
- Centralized Configuration (Spring Cloud Config)
- Circuit Breaker (Resilience4j)
- Distributed Tracing (Zipkin/Sleuth)

## Helm Charts

### 1. Creating Our Own Helm Chart & Template Files

Helm charts are organized in the following structure:
```
helm_charts/ 
├── charts/ # Dependencies 
├── templates/ # Template files 
│ ├── deployment.yaml 
│ ├── service.yaml 
│ ├── configmap.yaml 
│ └── ... 
├── Chart.yaml # Chart metadata 
├── values.yaml # Default configuration values 
└── values-{env}.yaml # Environment-specific overrides
```

Each Helm chart resides in its own directory under this repository. It contains the following components:

- **Chart.yaml** → Defines chart metadata (name, version, description).  
- **values.yaml** → Default configuration values (replicas, image details, env configs).  
- **templates/** → Kubernetes YAML manifests parameterized for Helm (Deployments, Services, ConfigMaps, Ingress, etc).


### 2. Helm Chart for Accounts Microservice

The Accounts service chart includes:
- Deployment configuration
- Service configuration
- ConfigMaps for application properties
- Resource limits and requests
- Liveness and readiness probes

### 3. Helm Charts for Other Microservices

Similar to the Accounts service, we've created Helm charts for:
- Cards Service
- Config Server
- Eureka Server
- Gateway Server
- Loans Service
- Message Service

### 4. Environment-Specific Configurations

Environment-specific configurations are managed using:
- `values-dev.yaml`: Development environment
- `values-qa.yaml`: QA environment
- `values-prod.yaml`: Production environment

## Installation

### 1. Install Keycloak

```bash
helm repo add bitnami [https://charts.bitnami.com/bitnami](https://charts.bitnami.com/bitnami)
helm install keycloak bitnami/keycloak \
  --set auth.adminUser=admin \
  --set auth.adminPassword=admin \
  --set postgresql.postgresqlPassword=postgres
```

### 2. Install Kafka

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install kafka bitnami/kafka
```

### 3. Install Prometheus & Grafana

```
# Add Prometheus repository
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

# Install kube-prometheus-stack
helm install prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace
```

### 4. Install Grafana Loki & Tempo

Loki: Centralized logging.
Tempo: Distributed tracing.
Deployment:
```
helm repo add grafana https://grafana.github.io/helm-charts
helm install loki grafana/loki-stack -n monitoring
helm install tempo grafana/tempo -n monitoring
```

## 🚀 Deployment Steps

### 1) Creating Our Own Helm Chart & Template Files
- Initialize a Helm chart:
  ```bash
  helm create accounts
  ``` 
Customize the generated templates/ to include Deployment, Service, Ingress, and ConfigMap.
Store parameterized values in values.yaml for flexible deployments.

### 2) Helm Chart for Accounts Microservice

Located inside /accounts directory.
Deploy using:
```
helm install accounts ./accounts -n banking-dev
```

### 3) Helm Charts for All Microservices

We have created Helm charts for the following microservices:
cards
configserver
eurekaserver
gatewayserver
loans
message

Each chart follows the same template structure (Deployment, Service, ConfigMap).
Example deployment:
```
helm install cards ./cards -n banking-dev
```

### 4) Environment-based Helm Charts (Dev, QA, Prod)

Separate values-dev.yaml, values-qa.yaml, and values-prod.yaml are maintained.
Example:
```
helm install accounts ./accounts -f values-qa.yaml -n banking-qa
```
This ensures different replicas, resource limits, and external URLs for each environment.

### 5) Deploy Microservices

```
# Add the chart repository
helm repo add mycharts https://sangramjit786.github.io/helm_comfig_repo/

# Install the charts
helm install accounts mycharts/accounts -f values-dev.yaml
helm install cards mycharts/cards -f values-dev.yaml
helm install loans mycharts/loans -f values-dev.yaml
# ... other services
```

### 6) Environment Setup

## dev
```
kubectl create namespace dev
helm install myapp ./helm_charts -f helm_charts/values-dev.yaml -n dev
```
## qa
```
kubectl create namespace qa
helm install myapp ./helm_charts -f helm_charts/values-qa.yaml -n qa
```
## prod
```
kubectl create namespace prod
helm install myapp ./helm_charts -f helm_charts/values-prod.yaml -n prod
```

## Troubleshooting
### Common Issues
  ### 1. Pods in CrashLoopBackOff:
    ```
    kubectl logs <pod-name> -n <namespace>
    kubectl describe pod <pod-name> -n <namespace>
    ```
  ### 2. Service Discovery Issues:
  
    - Verify Eureka server is running
    - Check service registration in Eureka dashboard
    
  ### 3. Configuration Issues:
   
    - Verify Config Server is running
    - Check application logs for configuration loading errors
