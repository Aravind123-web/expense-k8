
# HPA (Horizontal Pod Autoscaler) Setup

## Prerequisite

### 1. Install Metrics Server

First, install the Metrics Server in Kubernetes. The Metrics Server collects resource usage data such as CPU and memory utilization from pods and nodes. HPA uses these metrics to make scaling decisions.

Run the following command to install the Metrics Server:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## Deploy Expense-K8s Application

Make sure resource requests and limits are defined under the `resources` section of the deployment manifests.

### Step 1: Deploy MySQL

```bash
kubectl apply -f mysql/manifest.yaml
```

### Step 2: Check CPU and Memory Utilization

Verify that metrics are being collected successfully:

```bash
kubectl top pod
```

### Step 3: Deploy Backend

```bash
kubectl apply -f backend/manifest.yaml
```

### Step 4: Deploy Frontend

```bash
kubectl apply -f frontend/manifest.yaml
```

## Verification

Check pod resource utilization:

```bash
kubectl top pod
```

Check node resource utilization:

```bash
kubectl top node
```

## Notes

- Metrics Server must be installed and running before configuring HPA.
- HPA uses Metrics Server data to monitor CPU and memory utilization.
- Ensure that resource requests and limits are configured for all application deployments.
- Deploy the application components in the following order:
  1. MySQL
  2. Backend
  3. Frontend
