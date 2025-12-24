# Argo-Rollout

This repository contains resources and guidance for using Argo Rollouts.

## Quick install

Prerequisites:
- kubectl configured to talk to your Kubernetes cluster
- cluster with sufficient privileges to create namespaces and services

1. Create the namespace
```bash
kubectl create namespace argo-rollouts
```

2. Install the latest stable version of Argo Rollouts
```bash
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
```

## Rollouts dashboard

To install the Argo Rollouts dashboard and expose it:

1. Install the dashboard manifests:
```bash
kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-rollouts/master/manifests/dashboard-install.yaml -n argo-rollouts
```

2. (Optional) Change the dashboard Service to a NodePort so it is accessible externally:
```bash
kubectl patch svc argo-rollouts-dashboard -n argo-rollouts -p '{"spec": {"type": "NodePort"}}'
```

3. Find the NodePort assigned (if you used NodePort) and access via any cluster node IP:
```bash
kubectl get svc argo-rollouts-dashboard -n argo-rollouts
# Look for the NodePort in the output, then open: http://<NODE_IP>:<NODE_PORT>
```

Alternatively, use port forwarding (keeps the Service ClusterIP):
```bash
kubectl -n argo-rollouts port-forward svc/argo-rollouts-dashboard 3100:80
# Then open http://localhost:3100
```

## Notes

- If you're running on a managed Kubernetes provider that restricts NodePort usage, use port-forwarding or an Ingress to expose the dashboard.
- For production use, secure the dashboard behind authentication and TLS.
