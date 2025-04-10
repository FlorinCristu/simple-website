# Docker Build Pipeline Guide

This guide explains how to use the GitHub Actions pipeline to build and push Docker images to GitHub Container Registry.

## Setup Instructions

1. **Enable GitHub Container Registry for your repository**

   Go to your repository settings under "Packages" and ensure GitHub Container Registry is enabled.

2. **Set up required permissions**

   The workflow is already configured with the necessary permissions to push to the GitHub Container Registry using the `GITHUB_TOKEN` that's automatically available in GitHub Actions.

3. **How the pipeline works**

   The pipeline is triggered on:
   - Push to the main branch
   - Pull requests targeting the main branch
   - Manual trigger (workflow_dispatch)

   When triggered, it:
   - Checks out your code
   - Sets up Docker Buildx
   - Logs in to GitHub Container Registry
   - Extracts metadata for Docker image tags
   - Builds and pushes the Docker image to GitHub Container Registry

4. **Accessing your Docker images**

   After the workflow runs successfully, you can find your Docker images at:
   ```
   ghcr.io/YOUR_GITHUB_USERNAME/simple-website:latest
   ```

   Or with specific tags generated based on:
   - Git SHA (e.g., `ghcr.io/YOUR_GITHUB_USERNAME/simple-website:sha-123456789`)
   - Branch name (e.g., `ghcr.io/YOUR_GITHUB_USERNAME/simple-website:main`)

5. **Pulling your Docker image**

   ```bash
   docker pull ghcr.io/YOUR_GITHUB_USERNAME/simple-website:latest
   ```

6. **Running your Docker image locally**

   ```bash
   docker run -p 8080:80 ghcr.io/YOUR_GITHUB_USERNAME/simple-website:latest
   ```

   Then visit http://localhost:8080 in your browser.

## Customizing the Pipeline

To customize the pipeline, edit the `.github/workflows/docker-build.yml` file. Some common customizations:

- Add build arguments: Add `build-args: |` in the build-push-action section
- Change the Docker image name: Update the `images:` value in the metadata-action
- Add additional tags: Modify the `tags:` section in the metadata-action 