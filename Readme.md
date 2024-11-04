# Step by Step Guide to Create an Organization in Azure DevOps and Set Up a CI/CD Pipeline

## Prerequisites
- An Azure DevOps account.
- A GitHub repository and Docker setup.

---

## Creating a Resource Group for the Pipeline
1. Go to the Azure portal.
2. Create a resource group dedicated to your CI/CD pipeline.

---

## Configuring Azure Container Registry (ACR)
1. In Azure, create a Container Registry (ACR) for your project.
2. Assign the necessary permissions to users and services (e.g., admin-level access).

---

## Setting Up a New Project in Azure DevOps
1. Open Azure DevOps and create a new project:
   - Set visibility to private.
   - Choose Git for version control.
   - Configure the work process as required.

---

## Connecting Azure DevOps with ACR
1. Set up a secure connection to the Azure Container Registry.
2. Use a Service Principal for authentication.

---

## Adding the Project to GitHub and Setting as Source
1. Upload your source code to GitHub.
2. In Azure DevOps, configure GitHub as the source for the pipeline.

---

## Configuring the Build Pipeline
1. Create a new build pipeline in Azure DevOps.
2. Use the setup wizard to add these tasks:
   - Docker-related tasks to build your image.
   - Define image name and tag, using the ACR created earlier.
3. Save and run the pipeline.
4. Monitor execution in Azure DevOps and confirm image creation in Azure.

---

## Configuring the Release Pipeline
1. Disable classic pipelines in Azure DevOps settings if applicable.
2. Create a new release pipeline.
3. Set up stages for deploying to the Web App:
   - Choose cloud agents (e.g., Ubuntu) to run the stages.
   - Add deployment tasks to deploy the container, using scripts and configuration.
   - (Optional) Add approval steps for release management.

---

## Running the Release Pipeline
1. Commit changes to the GitHub repository to trigger the pipeline.
2. Monitor the release and verify the deployment in the Azure portal.

---

# AI Credit Analyzer
A Spring Boot application for analyzing and predicting credit using machine learning and the GPT API.

## Prerequisites
- **Java**: Version 17
- **Maven**: Version 3.8.x
- **Docker**: Latest version
- **Azure CLI**: Installed and configured

---

## Steps to Run the Application Locally
1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd checkpoint-2-devops
