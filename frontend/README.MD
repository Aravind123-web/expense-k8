
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


### Step 5: Create and Deploy the HPA Configuration

Create an HPA YAML file under the `frontend` directory.

In the HPA configuration:

- Set the frontend deployment name in `scaleTargetRef`.
- Set `minReplicas` to `1`.
- Set `maxReplicas` to `20`.
- Configure CPU utilization with `averageUtilization: 50`.

### Deploy the HPA

```bash
kubectl apply -f frontend/hpa.yaml
```

### Verify HPA

Check whether the HPA has been created successfully:

```bash
kubectl get hpa
```

View detailed HPA information:

```bash
kubectl describe hpa frontend
```

Monitor the current number of replicas and CPU utilization:

```bash
kubectl get hpa -w
```


## Notes

- Metrics Server must be installed and running before configuring HPA.
- HPA uses Metrics Server data to monitor CPU and memory utilization.
- Ensure that resource requests and limits are configured for all application deployments.
- Deploy the application components in the following order:
  1. MySQL
  2. Backend
  3. Frontend
- Ensure that the Metrics Server is installed and running before creating the HPA.
- The frontend deployment must have CPU resource requests and limits defined; otherwise, HPA cannot calculate CPU utilization correctly.
- HPA monitors the average CPU utilization of frontend pods.
- When the average CPU utilization exceeds **50%**, Kubernetes automatically scales out the deployment by increasing the number of replicas.
- When CPU utilization decreases, Kubernetes scales in the deployment by reducing the number of replicas.
- The number of replicas will always remain between **1** (`minReplicas`) and **20** (`maxReplicas`).
- Use `kubectl top pod` to verify that metrics are being collected correctly.
