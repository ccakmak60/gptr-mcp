# Rebuilding and Redeploying with the Health Check Fix

This guide explains how to pull the latest code changes (which include the fix for the `AttributeError` related to the health check in `server.py`), rebuild your Docker image, and redeploy it to Cloud Run.

### 1. Pull Latest Code Changes

To get the latest code, including the fix for the health check in `server.py`, open your terminal in the root directory of your project and run the following git command. If you've been working on a specific branch (like `main` or `master`, or the branch I might have created like `fix-healthcheck-route`), make sure you are on that branch.

```bash
# Ensure you are on the correct branch, e.g., main
# git checkout main

# Pull the latest changes from the remote repository
git pull
```

If `git pull` gives an error or doesn't fetch the latest changes, you might need to specify the remote (commonly `origin`) and the branch name:

```bash
# Example: git pull origin main
git pull origin <your-branch-name>
```
Replace `<your-branch-name>` with the actual name of the branch where the changes were committed.

### 2. Rebuild Docker Image with Cloud Build

Once you have the updated code, you need to rebuild your Docker image.
-   Refer to the detailed instructions in `CLOUD_BUILD_INSTRUCTIONS.md`.
-   The key command is:

```bash
gcloud builds submit --tag [REGION]-docker.pkg.dev/[PROJECT_ID]/[REPOSITORY_NAME]/[IMAGE_NAME]:[NEW_TAG] .
```

-   **VERY IMPORTANT**: Replace `[REGION]`, `[PROJECT_ID]`, `[REPOSITORY_NAME]`, and `[IMAGE_NAME]` with your actual values (e.g., `us-central1`, `your-project-id`, `gpt-researcher-repo`, `gpt-researcher-mcp`).
-   **USE A NEW IMAGE TAG** for `[NEW_TAG]` (e.g., `v1.1.1`, `fix-1`, or the first few characters of the latest commit hash). This ensures Cloud Run picks up the new version. For example: `us-central1-docker.pkg.dev/gold-gateway-280223/gpt-researcher-repo/gpt-worker:v1.1.1`
-   Run this command from the root directory of your project (where the `Dockerfile` is).

### 3. Redeploy to Cloud Run

After the new image is built and pushed to Artifact Registry, update your Cloud Run service to use it.
-   Refer to the detailed instructions in `CLOUD_RUN_DEPLOYMENT.md`.
-   The key command is:

```bash
gcloud run deploy [SERVICE_NAME] \
    --image [REGION]-docker.pkg.dev/[PROJECT_ID]/[REPOSITORY_NAME]/[IMAGE_NAME]:[NEW_TAG] \
    --platform managed \
    --region [CLOUD_RUN_REGION] \
    # Add all other parameters you used previously (port, env vars, etc.)
    # For example:
    # --port 8000 \
    # --set-env-vars "OPENAI_API_KEY=your-key,TAVILY_API_KEY=your-key,MCP_TRANSPORT=sse" \
    # --allow-unauthenticated
```
-   Replace `[SERVICE_NAME]` with your Cloud Run service name (e.g., `gpt-researcher-service`).
-   **Crucially, use the same `[NEW_TAG]` for the image URI that you used in the build step.**
-   Replace `[CLOUD_RUN_REGION]` with the region of your Cloud Run service.
-   Ensure all your necessary environment variables and other configurations are included as you had them in your previous successful deployment (before the health check issue arose, or from the `CLOUD_RUN_DEPLOYMENT.md` guide).

### 4. Verify Service

After deployment, check the Cloud Run service logs to ensure it starts without the `AttributeError` and that the health checks pass. Then test the service functionality.
