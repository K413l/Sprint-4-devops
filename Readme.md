Step-by-Step Guide to Create an Organization in Azure DevOps and Set Up a CI/CD Pipeline
Table of Contents
Prerequisites
Creating a Resource Group for the Pipeline
Configuring Azure Container Registry (ACR)
Setting Up a New Project in Azure DevOps
Connecting Azure DevOps with ACR
Adding the Project to GitHub and Setting as Source
Configuring the Build Pipeline
Configuring the Release Pipeline
Running the Release Pipeline
AI Credit Analyzer
Project Prerequisites
Steps to Run the Application Locally
Dockerization and Azure Deployment
API Endpoints
Endpoints for Analysis
Endpoints for User
Conclusion
Prerequisites
An Azure DevOps account.
A GitHub repository and Docker setup.
Creating a Resource Group for the Pipeline
Go to the Azure portal.
Create a resource group dedicated to your CI/CD pipeline.
Configuring Azure Container Registry (ACR)
In Azure, create a Container Registry (ACR) for your project.
Assign the necessary permissions to users and services (e.g., admin-level access).
Setting Up a New Project in Azure DevOps
Open Azure DevOps and create a new project:
Set visibility to Private.
Choose Git for version control.
Configure the work process as required.
Connecting Azure DevOps with ACR
Set up a secure connection to the Azure Container Registry.
Use a Service Principal for authentication.
Adding the Project to GitHub and Setting as Source
Upload your source code to GitHub.
In Azure DevOps, configure GitHub as the source for the pipeline.
Configuring the Build Pipeline
Create a new build pipeline in Azure DevOps.
Use the setup wizard to add these tasks:
Docker-related tasks to build your image.
Define image name and tag, using the ACR created earlier.
Save and run the pipeline.
Monitor execution in Azure DevOps and confirm image creation in Azure.
Configuring the Release Pipeline
Disable classic pipelines in Azure DevOps settings if applicable.
Create a new release pipeline.
Set up stages for deploying to the Web App:
Choose cloud agents (e.g., Ubuntu) to run the stages.
Add deployment tasks to deploy the container, using scripts and configuration.
(Optional) Add approval steps for release management.
Running the Release Pipeline
Commit changes to the GitHub repository to trigger the pipeline.
Monitor the release and verify the deployment in the Azure portal.
AI Credit Analyzer
This is a Spring Boot application for analyzing and predicting credit using machine learning and the GPT API.

Project Prerequisites
Java: Version 17
Maven: Version 3.8.x
Docker: Latest version
Azure CLI: Installed and configured
Steps to Run the Application Locally
Clone the Repository

bash
Copy code
git clone <repository-url>
cd checkpoint-2-devops
Build the Application

bash
Copy code
mvn clean package -DskipTests
Run the Application

bash
Copy code
java -jar target/aicreditanalyzer-0.0.1-SNAPSHOT.jar
Access the Application

The application will be available at http://localhost:8080.

Dockerization and Azure Deployment
Build and Push Docker Image to Azure
Login to Azure:

bash
Copy code
az login
Create a Resource Group:

bash
Copy code
az group create --name myResourceGroup --location eastus
Create Azure Container Registry (ACR):

bash
Copy code
az acr create --resource-group myResourceGroup --name rm551832acr --sku Basic
Login to ACR:

bash
Copy code
az acr login --name rm551832acr
Build Docker Image:

bash
Copy code
docker build -t aicreditanalyzer:latest .
Tag the Image:

bash
Copy code
docker tag aicreditanalyzer:latest rm551832acr.azurecr.io/rm551832-aicreditanalyzer:v1
Push the Image to ACR:

bash
Copy code
docker push rm551832acr.azurecr.io/rm551832-aicreditanalyzer:v1
Enable Admin Mode on ACR:

bash
Copy code
az acr update -n rm551832acr --admin-enabled true
Retrieve ACR Credentials

Username:

bash
Copy code
az acr credential show --name rm551832acr --query "username" --output tsv
Password:

bash
Copy code
az acr credential show --name rm551832acr --query "passwords[0].value" --output tsv
Create a Container Instance on Azure

bash
Copy code
az container create \
  --resource-group myResourceGroup \
  --name rm551832aci \
  --image rm551832acr.azurecr.io/rm551832-aicreditanalyzer:v1 \
  --cpu 1 --memory 1 \
  --registry-login-server rm551832acr.azurecr.io \
  --registry-username $ACR_USERNAME \
  --registry-password $ACR_PASSWORD \
  --ports 8080 \
  --dns-name-label rm551832aci
Get the Application URL

bash
Copy code
az container show \
  --resource-group myResourceGroup \
  --name rm551832aci \
  --query ipAddress.fqdn \
  --output tsv
The output URL will provide access to the running application.

API Endpoints
1. Endpoints for Analysis
Create a New Analysis (Credit Analysis or Prediction)
POST /credit-analysis or POST /credit-prediction

Example for credit analysis:

json
Copy code
{ "score": "750" }
Example for credit prediction:

json
Copy code
{ "amount": "5000", "income": "2000" }
Retrieve All Analyses
GET /credit-analysis or GET /credit-prediction
Update an Analysis
POST /analysis/update

Example for updating analysis:

json
Copy code
{
  "id": 1,
  "type": "analysis",
  "inputData": "800"
}
Delete an Analysis
GET /analysis/delete/{id}
2. Endpoints for User
Register a New User
POST /register

Example:

json
Copy code
{
  "name": "João Silva",
  "email": "joao.silva@example.com",
  "password": "senha123",
  "confirmPassword": "senha123"
}
User Login
POST /login

Example:

json
Copy code
{
  "email": "joao.silva@example.com",
  "password": "senha123"
}
Conclusion
This project showcases a complete CI/CD pipeline for a credit analysis application, using Docker and Azure for deployment.
