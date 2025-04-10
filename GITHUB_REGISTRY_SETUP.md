# GitHub Container Registry and Kubernetes Deployment Setup

This guide explains how to set up GitHub Container Registry and deploy your application to Kubernetes.

## GitHub Container Registry Setup

1. **Update your repository name in deployment files**

   In `clusters/one/deployment.yaml`, replace `GITHUB_USERNAME` with your actual GitHub username:

   ```yaml
   image: ghcr.io/GITHUB_USERNAME/simple-website:latest
   ```

   Example:
   ```yaml
   image: ghcr.io/johndoe/simple-website:latest
   ```

2. **Enable GitHub Actions**

   Ensure GitHub Actions is enabled for your repository under the "Actions" tab.

3. **Create a Personal Access Token (PAT) for GitHub Container Registry**

   If you need to pull the image from a private registry in your Kubernetes cluster:

   - Go to GitHub Settings > Developer settings > Personal access tokens
   - Generate a new token with the `read:packages` scope
   - Use this token in your Kubernetes cluster to pull images

## Kubernetes Deployment

1. **Apply the Kubernetes resources**

   ```bash
   kubectl apply -k clusters/one/
   ```

2. **For private repositories, create a Kubernetes Secret**

   ```bash
   kubectl create secret docker-registry github-registry \
     --namespace simple-website \
     --docker-server=ghcr.io \
     --docker-username=YOUR_GITHUB_USERNAME \
     --docker-password=YOUR_GITHUB_PAT \
     --docker-email=YOUR_EMAIL@example.com
   ```

   Then add this to your deployment:

   ```yaml
   spec:
     template:
       spec:
         imagePullSecrets:
         - name: github-registry
   ```

## Workflow

The complete workflow for building and deploying is:

1. Make changes to your application
2. Commit and push changes to GitHub
3. GitHub Actions builds and pushes the image to GitHub Container Registry
4. Update your Kubernetes deployment if needed
5. Apply the changes to your Kubernetes cluster

## Testing the deployment

Check if your deployment is running:

```bash
kubectl get pods -n simple-website
```

Port-forward to access your website:

```bash
kubectl port-forward svc/simple-website 8080:80 -n simple-website
```

Then visit http://localhost:8080 in your browser. 