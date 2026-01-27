# SMS Spam Detection Application - Deployment Architecture

## Table of Contents
- [Overview](#overview)
- [Architecture Diagram](#architecture-diagram)
- [Deployment Components](#deployment-components)
- [Request Flow](#request-flow)
- [Traffic Management & Experimentation](#traffic-management--experimentation)
- [Monitoring & Observability](#monitoring--observability)
- [Access Configuration](#access-configuration)
- [Current Limitations & Future Work](#current-limitations--future-work)

## Overview

The SMS Spam Detection Application is deployed on a Kubernetes cluster with Istio service mesh for advanced traffic management. The deployment supports A/B testing with a 90/10 traffic split between the stable and new versions.

**Repository**: [doda25-team16/operation](https://github.com/doda25-team16/operation)

### Core Services
- **sms-app**: Java Spring Boot API Gateway (user interface)
- **sms-model**: Python Flask ML inference service

### Infrastructure Stack
- **Orchestration**: Kubernetes (v1.32)
- **Service Mesh**: Istio (v1.28.0)
- **Ingress**: Nginx Ingress Controller
- **Load Balancer**: MetalLB
- **Monitoring**: Prometheus Operator
- **Dashboard**: Kubernetes Dashboard
- **Deployment**: Helm 3

## Architecture Diagram

![Architecture Diagram](Architecture.png)

The diagram above illustrates the two distinct traffic paths into the cluster:

1.  **Standard Access (Left)**: Users access `http://team16-sms.local` via the Nginx Ingress. This routes traffic to the Kubernetes Service, which performs standard load balancing across all available pods.
2.  **Istio Routing (Right)**: Users access `http://192.168.56.91` via the Istio Gateway. This path flows through the **VirtualService**, which actively manages traffic and enforces the **90/10 split** between the stable (`latest`) and new (`v1`) versions.

## Deployment Components

### 1. Load Balancing & Ingress Layer

#### MetalLB (IP: 192.168.56.90-192.168.56.99)
- **192.168.56.90**: Assigned to Nginx Ingress Controller
- **192.168.56.91**: Assigned to Istio Ingress Gateway

#### Nginx Ingress Controller
- **Access Point**: `http://team16-sms.local` (mapped to `.90`)
- **Routing**: Defined in `k8s/ingress.yml`
- **Behavior**: Routes traffic to the `sms-app` Kubernetes Service on port 8080.

### 2. Service Mesh Layer

#### Istio Gateway (my-gateway)
- **Access Point**: `http://192.168.56.91`
- **Configuration**: Defined in `k8s/istio.yml`
- **Purpose**: Alternative entry point that allows Istio to manage traffic ingress directly.

#### Istio VirtualService (my-entry-service)
- **Purpose**: Implements intelligent traffic routing and A/B testing
- **Traffic Split**:
  - 90% → `sms-app` subset `latest` (stable version)
  - 10% → `sms-app` subset `v1` (new version)

#### Istio DestinationRules
Two DestinationRules define version subsets:
- **sms-app-dr**: Maps `latest` and `v1` subsets to pods with corresponding version labels.
- **sms-model-dr**: Maps `latest` and `v1` subsets to pods with corresponding version labels.

### 3. Application Layer

#### SMS App Service (sms-app)
- **Type**: ClusterIP
- **Port**: 8080
- **Deployments**: `sms-app-latest` and `sms-app-v1`
- **Env Variables**: Configured via `sms-app-config` ConfigMap and `sms-app-secret` Secret.

#### SMS Model Service (sms-model)
- **Type**: ClusterIP
- **Port**: 8081
- **Deployments**: `sms-model-latest` and `sms-model-v1`

### 4. Monitoring Stack 

Monitoring begins at the frontend, where a variety of metrics are gathered and are visible through `sms/metrics`. We use Prometheus to gather the data from this endpoint at an interval of 1 second (This was decided upon on the `monitoring.yml` file). It is important to note that once this data is gathered by Prometheus it can be viewed on the Prometheus URL by searching up the specific data you would like to view. However, we have implemented a Grafana dashboard (`grafana-dashboard-configmap.yml`) which takes the gathered Prometheus data and displays it in several different ways depending on what it is meant to show. An alert manager has also been setup in `alertmanager.yl` that gathers the prometheus data and alerts users based on that data.

#### Metrics
\- `button_clicked`: Number of "Send" button clicked.
\- `total_requests`: Total number of requests sent to the model. This metrics can be filtered by the resultant response type "ham", "spam", and "failed".
\- `active_requests`: The number of requests currently being processing by the model.

### 5. Management Interface

#### Kubernetes Dashboard
- **URL**: `http://dashboard.local`
- **Ingress**: Configured in `ansible/finalization.yaml`
- **Backend**: Proxies to `kubernetes-dashboard-kong-proxy` on port 443.

## Traffic Management & Experimentation

![Istio Traffic Management](images/Istio%20Diagram.png)

### A/B Testing Configuration
The traffic split is configured in `k8s/istio.yml`. The `VirtualService` defines weighted routing rules that distribute incoming requests between the two deployment subsets.

To modify the split (e.g., to 90/10), edit the weights in `k8s/istio.yml` and re-apply the file:
```yaml
    - destination: { host: sms-app, subset: latest }
      weight: 90
    - destination: { host: sms-app, subset: v1 }
      weight: 10
```

# Deployment Commands

## Option 1: Using Helm (Recommended)

This command installs the entire application stack using the chart located in `helm-chart/sms-app`.

In accordance to the rubric for A3, we want a pre-deployed secret:
```bash
kubectl -n default create secret generic sms-app-secret \
  --from-literal=MY_SECRET=YmFy
```

Install helm sms-app with the secret reference:
```bash
helm upgrade --install sms-app ./helm-chart/sms-app \
  -n default \
  --set app.secret.secretName=sms-app-secret
```
---
Normally:
```bash
helm install sms-app ./helm-chart/sms-app
```

## Option 2: Using kubectl
This command applies all Kubernetes manifests in the `k8s/` directory.

```bash
    # 1. Apply Configurations and Secrets
    kubectl apply -f k8s/configmap.yml
    kubectl apply -f k8s/secret.yml

    # 2. Apply Core Services & Deployments
    kubectl apply -f k8s/sms-app-deployment.yml
    kubectl apply -f k8s/sms-app-service.yml
    kubectl apply -f k8s/sms-model-deployment.yml
    kubectl apply -f k8s/sms-model-service.yml

    # 3. Apply Networking & Service Mesh Configuration
    kubectl apply -f k8s/ingress.yml
    kubectl apply -f k8s/istio.yml

    # 4. Apply Monitoring Stack
    kubectl apply -f k8s/monitoring.yml
```



