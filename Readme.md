
# AI Credit Analyzer

This is a Spring Boot application that analyzes and predicts credit using machine learning and integrations with GPT API.

## Prerequisites

Before running this project, ensure you have the following installed:

- Java 17
- Maven 3.8.x
- Docker
- Azure CLI

## Steps to Run the Application Locally

1. **Clone the Repository**

   ```bash
   git clone <repository-url>
   cd checkpoint-2-devops
   ```

2. **Build the Application**

   ```bash
   mvn clean package -DskipTests
   ```

3. **Run the Application**

   ```bash
   java -jar target/aicreditanalyzer-0.0.1-SNAPSHOT.jar
   ```

4. **Access the Application**

   The application will run at `http://localhost:8080`.

## Dockerization and Azure Deployment

### Build and Push Docker Image to Azure

1. **Login to Azure**

   ```bash
   az login
   ```

2. **Create a Resource Group**

   ```bash
   az group create --name myResourceGroup --location eastus
   ```

3. **Create Azure Container Registry (ACR)**

   ```bash
   az acr create --resource-group myResourceGroup --name rm551832acr --sku Basic
   ```

4. **Login to ACR**

   ```bash
   az acr login --name rm551832acr
   ```

5. **Build Docker Image**

   ```bash
   docker build -t aicreditanalyzer:latest .
   ```

6. **Tag the Image**

   ```bash
   docker tag aicreditanalyzer:latest rm551832acr.azurecr.io/rm551832-aicreditanalyzer:v1
   ```

7. **Push the Image to ACR**

   ```bash
   docker push rm551832acr.azurecr.io/rm551832-aicreditanalyzer:v1
   ```

8. **Enable Admin Mode on ACR**

   ```bash
   az acr update -n rm551832acr --admin-enabled true
   ```

9. **Retrieve ACR Credentials**

   1. **Retrieve the ACR Username**:

      ```bash
      az acr credential show --name rm551832acr --query "username" --output tsv
      ```

      Copy the returned username and set it as an environment variable:

      ```cmd
      set ACR_USERNAME=coloque_o_username_aqui
      ```

   2. **Retrieve the ACR Password**:

      ```bash
      az acr credential show --name rm551832acr --query "passwords[0].value" --output tsv
      ```

      Copy the returned password and set it as an environment variable:

      ```cmd
      set ACR_PASSWORD=coloque_a_senha_aqui
      ```

10. **Create Container Instance on Azure**

    ```bash
    az container create --resource-group myResourceGroup --name rm551832aci     --image rm551832acr.azurecr.io/rm551832-aicreditanalyzer:v1     --cpu 1 --memory 1     --registry-login-server rm551832acr.azurecr.io     --registry-username %ACR_USERNAME%     --registry-password %ACR_PASSWORD%     --ports 8080 --dns-name-label rm551832aci
    ```

11. **Get the Application URL**

    ```bash
    az container show --resource-group myResourceGroup --name rm551832aci --query ipAddress.fqdn --output tsv
    ```

    The output will provide the URL where the application is running, e.g., `rm551832aci.eastus.azurecontainer.io`.

## API Endpoints

### 1. Endpoints for `Analysis`

#### a. Create a New Analysis (Credit Analysis or Prediction)

**Endpoint:** `POST /credit-analysis` or `POST /credit-prediction`

- Example JSON for credit analysis:

    ```json
    {
      "score": "750"
    }
    ```

- Example JSON for credit prediction:

    ```json
    {
      "amount": "5000",
      "income": "2000"
    }
    ```

#### b. Retrieve All Analyses

**Endpoint:** `GET /credit-analysis` or `GET /credit-prediction`

Returns a list of analyses. No JSON body is required for this operation.

#### c. Update an Analysis

**Endpoint:** `POST /analysis/update`

- Example JSON for updating an analysis:

    ```json
    {
      "id": 1,
      "type": "analysis",
      "inputData": "800"
    }
    ```

- Example JSON for updating a prediction:

    ```json
    {
      "id": 2,
      "type": "prediction",
      "amount": "6000",
      "income": "2500"
    }
    ```

#### d. Delete an Analysis

**Endpoint:** `GET /analysis/delete/{id}`

Replace `{id}` with the ID of the analysis you wish to delete.

### 2. Endpoints for `User`

#### a. Register a New User

**Endpoint:** `POST /register`

- Example JSON for user registration:

    ```json
    {
      "name": "João Silva",
      "email": "joao.silva@example.com",
      "password": "senha123",
      "confirmPassword": "senha123"
    }
    ```

#### b. User Login

**Endpoint:** `POST /login`

- Example JSON for login:

    ```json
    {
      "email": "joao.silva@example.com",
      "password": "senha123"
    }
    ```

## Conclusion

This project demonstrates a full pipeline for analyzing and predicting credit using machine learning, leveraging Docker for containerization, and deploying on Azure Container Instances.
