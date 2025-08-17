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

## 🚀 Deployment Steps

### 1) Creating Our Own Helm Chart & Template Files
- Initialize a Helm chart:
  ```bash
  helm create accounts
  ``` 
Customize the generated templates/ to include Deployment, Service, Ingress, and ConfigMap.

Store parameterized values in values.yaml for flexible deployments.
