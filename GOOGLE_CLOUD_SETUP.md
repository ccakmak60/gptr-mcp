# Google Cloud Project Setup for Cloud Run

This guide will walk you through setting up your Google Cloud project to deploy applications using Cloud Run.

## 1. Create or Select a Google Cloud Project

*   **Go to the Google Cloud Console:** Open your web browser and navigate to [https://console.cloud.google.com/](https://console.cloud.google.com/).
*   **Select an Existing Project:**
    *   If you have an existing Google Cloud Project you want to use, locate the project selector at the top of the page (it might display the name of your currently selected project).
    *   Click on the project selector and choose your desired project from the list.
*   **Create a New Project (if needed):**
    *   If you don't have a project or want to create a new one, click on the project selector at the top of the page.
    *   In the "Select a project" dialog, click on "**NEW PROJECT**".
    *   Enter a descriptive **Project name**.
    *   Select a **Billing account** if prompted (you can also link this later, but it's required to use services).
    *   Choose an **Organization** and **Location** if applicable.
    *   Click "**CREATE**".

## 2. Enable Billing

*   **Important:** A billing account must be linked to your project to use Google Cloud services, including Cloud Run.
*   **Navigate to Billing:**
    *   In the Google Cloud Console, click the **Navigation Menu** (the three horizontal lines in the top-left corner).
    *   Select "**Billing**".
*   **Link a Billing Account:**
    *   If your project doesn't have a billing account linked, you'll see a prompt to "**Link a billing account**" or "**Manage billing accounts**".
    *   Follow the on-screen instructions to either select an existing billing account or create a new one. You'll need to provide payment information if setting up a new billing account.

## 3. Enable Necessary APIs

To use Cloud Run and related services, you need to enable specific APIs for your project.

*   **Go to the API Library:**
    *   In the Google Cloud Console, click the **Navigation Menu**.
    *   Go to "**APIs & Services**".
    *   Select "**Library**".
*   **Enable the Cloud Run API:**
    *   In the API Library search bar, type "**Cloud Run API**" and press Enter.
    *   Click on "**Cloud Run API**" in the search results.
    *   If the API is not already enabled, click the "**ENABLE**" button. Wait for the process to complete.
*   **Enable the Cloud Build API:**
    *   Go back to the API Library (Navigation Menu > APIs & Services > Library).
    *   In the search bar, type "**Cloud Build API**" and press Enter.
    *   Click on "**Cloud Build API**" in the search results.
    *   If the API is not already enabled, click the "**ENABLE**" button. Wait for the process to complete.
*   **Enable the Artifact Registry API:**
    *   Go back to the API Library.
    *   In the search bar, type "**Artifact Registry API**" and press Enter.
    *   Click on "**Artifact Registry API**" in the search results.
    *   If the API is not already enabled, click the "**ENABLE**" button. Wait for the process to complete.

Once these steps are completed, your Google Cloud project will be configured with the basic requirements for deploying applications with Cloud Run.
