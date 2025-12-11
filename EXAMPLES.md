# Kubernetes Examples - Quick Reference

## Resources Created

### 1. Three Individual Pods

#### Pod 1: Nginx Web Server
- **File**: `k8s/pods/pod1-nginx.yaml`
- **Image**: nginx:1.25
- **Port**: 80
- **Resources**: 64Mi-128Mi RAM, 250m-500m CPU

#### Pod 2: BusyBox
- **File**: `k8s/pods/pod2-busybox.yaml`
- **Image**: busybox:1.36
- **Command**: Prints "Hello Kubernetes!" and sleeps
- **Resources**: 32Mi-64Mi RAM, 100m-200m CPU

#### Pod 3: Redis Cache
- **File**: `k8s/pods/pod3-redis.yaml`
- **Image**: redis:7-alpine
- **Port**: 6379
- **Features**: Environment variables, persistent volume
- **Resources**: 128Mi-256Mi RAM, 250m-500m CPU

### 2. Deployment with 3 Replicas

- **File**: `k8s/deployments/deployment.yaml`
- **Name**: webapp-deployment
- **Replicas**: 3
- **Image**: nginx:1.25
- **Features**:
  - Liveness probe (HTTP GET /)
  - Readiness probe (HTTP GET /)
  - Resource limits
  - Environment variables

### 3. Additional Resources

#### Service (LoadBalancer)
- **File**: `k8s/services/service.yaml`
- **Type**: LoadBalancer
- **Port**: 80
- **Target**: webapp deployment

#### ConfigMap
- **File**: `k8s/configs/configmap.yaml`
- **Contains**: Database URL, cache settings, log level, app properties

#### Namespace
- **File**: `k8s/configs/namespace.yaml`
- **Name**: development
- **Purpose**: Environment isolation

## Quick Start Commands

```bash
# Apply all resources at once
kubectl apply -f k8s/pods/
kubectl apply -f k8s/deployments/
kubectl apply -f k8s/services/
kubectl apply -f k8s/configs/

# Or apply everything recursively
kubectl apply -f k8s/ -R

# Check status
kubectl get all
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get configmaps
kubectl get namespaces
```

## Architecture Overview

```
┌─────────────────────────────────────────┐
│         Kubernetes Cluster              │
│                                         │
│  ┌───────────┐  ┌───────────┐         │
│  │ Namespace │  │ ConfigMap │         │
│  └───────────┘  └───────────┘         │
│                                         │
│  ┌─────────────────────────────────┐  │
│  │      LoadBalancer Service       │  │
│  └────────────┬────────────────────┘  │
│               │                        │
│  ┌────────────▼────────────────────┐  │
│  │    Deployment (3 replicas)      │  │
│  │  ┌─────┐  ┌─────┐  ┌─────┐     │  │
│  │  │Pod  │  │Pod  │  │Pod  │     │  │
│  │  │nginx│  │nginx│  │nginx│     │  │
│  │  └─────┘  └─────┘  └─────┘     │  │
│  └─────────────────────────────────┘  │
│                                         │
│  Individual Pods (not in deployment):  │
│  ┌─────────┐ ┌──────────┐ ┌────────┐ │
│  │  nginx  │ │ busybox  │ │ redis  │ │
│  │   Pod   │ │   Pod    │ │  Pod   │ │
│  └─────────┘ └──────────┘ └────────┘ │
└─────────────────────────────────────────┘
```
