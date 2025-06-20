# Deploying Your Docker Image to Cloud Run

This guide provides the `gcloud` command to deploy your Docker image from Artifact Registry to a new Cloud Run service.

**Prerequisites:**

*   You have successfully pushed your Docker image to Artifact Registry.
*   You have the full URI of your Docker image (e.g., `[REGION]-docker.pkg.dev/[PROJECT_ID]/[REPOSITORY_NAME]/[IMAGE_NAME]:[TAG]`).
*   You have `gcloud` CLI installed and authenticated with permissions to deploy Cloud Run services and access Artifact Registry.
*   Your Google Cloud project has the Cloud Run API enabled.

## Deploy to Cloud Run Command

Execute the following command in your terminal, replacing the placeholders with your specific values.

```bash
gcloud run deploy [SERVICE_NAME] \
    --image [IMAGE_URI] \
    --platform managed \
    --region [REGION] \
    --port 8000 \
    --set-env-vars "OPENAI_API_KEY=[YOUR_OPENAI_API_KEY],MCP_TRANSPORT=sse,TAVILY_API_KEY=[YOUR_TAVILY_API_KEY]" \
    --allow-unauthenticated \
    --min-instances 0 \
    --max-instances 2 \
    --cpu 1 \
    --memory 512Mi \
    --concurrency 80
```

**Explanation of Placeholders and Parameters:**

*   `[SERVICE_NAME]`:
    *   Choose a name for your new Cloud Run service. This will be part of the service's URL.
    *   *Example:* `gpt-researcher-service`, `my-web-app`

*   `[IMAGE_URI]`:
    *   This is the full path to your Docker image in Artifact Registry. You should have noted this from the Cloud Build step.
    *   *Format:* `[REGION]-docker.pkg.dev/[PROJECT_ID]/[REPOSITORY_NAME]/[IMAGE_NAME]:[TAG]`
    *   *Example:* `us-central1-docker.pkg.dev/my-gcp-project-123/gpt-researcher-repo/gpt-researcher-mcp:latest`

*   `--platform managed`:
    *   Specifies that you are deploying to the fully managed Cloud Run environment, where Google handles the underlying infrastructure.

*   `[REGION]`:
    *   The Google Cloud region where you want to deploy your service.
    *   *Recommendation:* Use the same region as your Artifact Registry repository to potentially reduce latency and costs.
    *   *Example:* `us-central1`, `europe-west1`

*   `--port 8000`:
    *   The port number that your container application listens on for incoming requests. This should match the port exposed in your `Dockerfile` (e.g., `EXPOSE 8000`).
    *   Our `gpt-researcher-mcp` server is configured to listen on port 8000 when in SSE mode.

*   `--set-env-vars "OPENAI_API_KEY=[YOUR_OPENAI_API_KEY],MCP_TRANSPORT=sse,TAVILY_API_KEY=[YOUR_TAVILY_API_KEY]"`:
    *   Sets environment variables for your Cloud Run service. Variables are comma-separated.
    *   `OPENAI_API_KEY=[YOUR_OPENAI_API_KEY]`:
        *   **CRITICAL:** Replace `[YOUR_OPENAI_API_KEY]` with your actual OpenAI API key. The application will not work without it.
    *   `TAVILY_API_KEY=[YOUR_TAVILY_API_KEY]`:
        *   **CRITICAL:** Replace `[YOUR_TAVILY_API_KEY]` with your actual Tavily API key if you are using Tavily for search. The application may not work as expected or at all for research tasks without it.
    *   `MCP_TRANSPORT=sse`:
        *   This explicitly sets the transport mode to Server-Sent Events (SSE), which is necessary for the Cloud Run deployment as STDIO is not suitable. While the `Dockerfile` also sets this, including it here ensures the correct mode.

*   `--allow-unauthenticated`:
    *   This flag makes your Cloud Run service publicly accessible over the internet. Anyone with the service URL can access it.
    *   **Security Note:** If your service should be private and only accessible by other Google Cloud services or authenticated users, use `--no-allow-unauthenticated`. You would then need to configure IAM permissions or other authentication methods to access the service.

*   `--min-instances 0`:
    *   Allows Cloud Run to scale your service down to zero instances when there are no incoming requests. This is cost-effective for services with intermittent traffic. The service will quickly scale up when a new request arrives (cold start).

*   `--max-instances 2`:
    *   Sets the maximum number of container instances that Cloud Run can scale up to. Adjust this value based on your expected peak load and budget.
    *   *Example:* `5`, `10`

*   `--cpu 1`:
    *   Allocates 1 virtual CPU to each container instance. You can specify fractional CPUs or more CPUs.
    *   *Example:* `0.5`, `2`

*   `--memory 512Mi`:
    *   Allocates 512 MiB of memory to each container instance.
    *   *Example:* `256Mi`, `1Gi`

*   `--concurrency 80`:
    *   The maximum number of concurrent requests that a single container instance can handle before Cloud Run attempts to scale up (if not at max-instances). The default is 80. Adjust based on your application's performance characteristics.

**Example Command:**

If your details are:
*   Service Name: `gpt-researcher-service`
*   Image URI: `us-central1-docker.pkg.dev/my-gcp-project-123/gpt-researcher-repo/gpt-researcher-mcp:latest`
*   Region: `us-central1`
*   OpenAI API Key: `sk-xxxxxxxxxxxxxxxxxxxxxx`
*   Tavily API Key: `tvly-yyyyyyyyyyyyyyyyyyyyyy`

The command would be:

```bash
gcloud run deploy gpt-researcher-service \
    --image us-central1-docker.pkg.dev/my-gcp-project-123/gpt-researcher-repo/gpt-researcher-mcp:latest \
    --platform managed \
    --region us-central1 \
    --port 8000 \
    --set-env-vars "OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxx,MCP_TRANSPORT=sse,TAVILY_API_KEY=tvly-yyyyyyyyyyyyyyyyyyyyyy" \
    --allow-unauthenticated \
    --min-instances 0 \
    --max-instances 2 \
    --cpu 1 \
    --memory 512Mi \
    --concurrency 80
```

**Adjusting Performance Settings:**

You can adjust `--cpu`, `--memory`, `--min-instances`, `--max-instances`, and `--concurrency` based on your application's needs and performance testing. Start with reasonable values and monitor your service's performance and cost in the Google Cloud Console.

After running the command, `gcloud` will deploy your service. Once completed, it will output the URL of your deployed Cloud Run service. You can then access your application at this URL. You can also view and manage your service in the Google Cloud Console under "Cloud Run".
