# Artifact Registry Setup for Docker Images

This guide explains how to create a Docker repository in Google Cloud Artifact Registry. This repository will be used to store your Docker images, which can then be deployed to services like Cloud Run.

## 1. Navigate to Artifact Registry

*   **Using the Search Bar (Recommended):**
    *   In the Google Cloud Console, locate the search bar at the top of the page.
    *   Type "**Artifact Registry**" and press Enter.
    *   Select "Artifact Registry" from the search results.
*   **Using the Navigation Menu:**
    *   Click the **Navigation Menu** (the three horizontal lines in the top-left corner).
    *   Scroll down or filter to find "**Artifact Registry**" (it might be under "CI/CD" or a similar category).
    *   Click on it.

## 2. Create a Repository

*   Once in the Artifact Registry section, click the "**CREATE REPOSITORY**" button. If you have existing repositories, this button might be located above the list of repositories.
*   **Configure the new repository:**
    *   **Name:** Enter a unique name for your repository.
        *   *Example:* `gpt-researcher-repo`
    *   **Format:** Select "**Docker**" from the dropdown menu. This specifies that the repository will store Docker container images.
    *   **Mode:** Keep the default "Standard" mode unless you have specific requirements for a "Remote" or "Virtual" repository.
    *   **Location Type:** Choose "**Region**".
    *   **Region:** Select a region for your repository.
        *   *Recommendation:* Choose the same region where you plan to run your Cloud Run services or other resources that will access this repository (e.g., `us-central1`, `europe-west1`). This can reduce latency and costs.
    *   **Description:** (Optional) Add a description for your repository.
    *   **Labels:** (Optional) Add any labels if you use them for organizing resources.
    *   **Encryption:** Leave the "Google-managed encryption key" selected unless you have specific requirements to use a "Customer-managed encryption key (CMEK)".
*   Click the "**CREATE**" button at the bottom of the page.

## 3. Note the Repository Path

*   After the repository is successfully created, you will be taken to its details page or back to the list of repositories.
*   **Identify your repository path:** The path to your Docker repository is crucial for pushing and pulling images. It follows this structure:

    `[REGION]-docker.pkg.dev/[YOUR_PROJECT_ID]/[YOUR_REPOSITORY_NAME]`

*   **Example:** If you chose:
    *   Region: `us-central1`
    *   Your Project ID is: `my-gcp-project-123`
    *   Your Repository Name is: `gpt-researcher-repo`

    Then your repository path would be:

    `us-central1-docker.pkg.dev/my-gcp-project-123/gpt-researcher-repo`

*   **Important:** Carefully note this full repository path. You will need it in subsequent steps, such as when tagging your Docker image before pushing it to Artifact Registry.

You have now successfully created a Docker repository in Artifact Registry. You can proceed to build your Docker image and push it to this repository.
