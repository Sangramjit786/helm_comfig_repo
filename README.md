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
