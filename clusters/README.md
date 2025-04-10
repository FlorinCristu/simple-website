# GitOps with Flux - Cluster Configuration

This directory contains Kubernetes manifests for deploying applications via GitOps using Flux.

## Structure

- `one/`: Contains the manifests for the "one" cluster
  - `namespace.yaml`: Creates the application namespace
  - `deployment.yaml`: Deploys the application pods
  - `service.yaml`: Creates a service to access the application
  - `configmap.yaml`: Stores the website content
  - `ingress.yaml`: Configures external access
  - `hpa.yaml`: Horizontal Pod Autoscaler for the deployment
  - `kustomization.yaml`: Kustomize configuration for managing all resources

## How to Use

1. Make changes to the manifests in the appropriate cluster directory
2. Commit and push your changes to the repository
3. Flux will automatically detect and apply the changes to your cluster

## Testing

To test the deployment locally, you can use:

```bash
kubectl kustomize clusters/one
```

To manually apply the changes (though Flux should do this automatically):

```bash
kubectl apply -k clusters/one
``` 