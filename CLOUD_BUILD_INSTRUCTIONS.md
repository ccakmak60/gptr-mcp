# Building and Pushing Docker Image using Google Cloud Build

This guide provides the `gcloud` commands to configure Docker, build your Docker image using Google Cloud Build, and push it to your Artifact Registry repository.

**Prerequisites:**

*   You have a Google Cloud Project set up with billing enabled.
*   You have enabled the Cloud Build API and Artifact Registry API.
*   You have created an Artifact Registry Docker repository.
*   You have `gcloud` CLI installed and authenticated.
*   You have a `Dockerfile` in the root directory of your project.

**Run these commands from the root directory of your project.**

## 1. Configure Docker Credential Helper

This command configures Docker to use `gcloud` as a credential helper. This allows Docker to authenticate with Artifact Registry using your Google Cloud credentials, so you don't have to manage Docker credentials manually.

```bash
gcloud auth configure-docker [REGION]-docker.pkg.dev
```

*   **Replace `[REGION]`** with the region of your Artifact Registry repository (e.g., `us-central1`, `europe-west1`).

    *Example:*
    ```bash
    gcloud auth configure-docker us-central1-docker.pkg.dev
    ```

After running this command, Docker will be able to push and pull images from your Artifact Registry repository in the specified region.

## 2. Build and Push the Image with Cloud Build

This command uses Google Cloud Build to build your Docker image and then pushes it to your Artifact Registry repository. Cloud Build looks for a `Dockerfile` in your current directory (the build context).

```bash
gcloud builds submit --tag [REGION]-docker.pkg.dev/[PROJECT_ID]/[REPOSITORY_NAME]/[IMAGE_NAME]:[TAG] .
```

**Explanation of Placeholders:**

*   `[REGION]`: The Google Cloud region where your Artifact Registry repository is located.
    *   *Example:* `us-central1`
*   `[PROJECT_ID]`: Your unique Google Cloud Project ID. You can find this in the Google Cloud Console.
    *   *Example:* `my-gcp-project-123`
*   `[REPOSITORY_NAME]`: The name you gave to your Artifact Registry repository.
    *   *Example:* `gpt-researcher-repo`
*   `[IMAGE_NAME]`: A name for your Docker image.
    *   *Example:* `gpt-researcher-mcp` or `my-app`
*   `[TAG]`: A tag for your image version. Common tags include `latest`, `v1.0`, or a specific commit SHA.
    *   *Example:* `latest`, `v1.0.1`
*   `.` (at the end): This dot signifies that the build context (the source code and `Dockerfile` for building the image) is the current directory. Ensure you run this command from the root of your project where the `Dockerfile` is located.

**Complete Example:**

If your details are:
*   Region: `us-central1`
*   Project ID: `my-gcp-project-123`
*   Repository Name: `gpt-researcher-repo`
*   Image Name: `gpt-researcher-mcp`
*   Tag: `latest`

The command would be:

```bash
gcloud builds submit --tag us-central1-docker.pkg.dev/my-gcp-project-123/gpt-researcher-repo/gpt-researcher-mcp:latest .
```

After running this command, Cloud Build will:
1.  Upload your project files (the build context) to a Cloud Storage bucket.
2.  Execute the Docker build using the `Dockerfile` found in the build context.
3.  Tag the built image with the full path you specified.
4.  Push the tagged image to your Artifact Registry repository.

You can monitor the build progress in the Google Cloud Console under "Cloud Build". Once completed, your image will be available in Artifact Registry.
