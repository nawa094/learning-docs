### **CI/CD Pipeline Examples for Deploying a Basic .NET Web API Application**

This repository provides two practical examples of setting up CI/CD pipelines to deploy a basic .NET Web API application:

1. **Azure Pipelines**: Automating build and deployment using Azure DevOps.
2. **GitHub Actions**: Automating build and deployment using GitHub Actions.

These examples are designed to help you understand how to implement Continuous Integration and Continuous Deployment (CI/CD) for .NET applications in two popular platforms.

---

## **Table of Contents**

- [**Table of Contents**](#table-of-contents)
- [**Prerequisites**](#prerequisites)
- [**Repository Structure**](#repository-structure)
- [**Example 1: Azure Pipelines**](#example-1-azure-pipelines)
  - [**Steps**](#steps)
- [**Example 2: GitHub Actions**](#example-2-github-actions)
  - [**Steps**](#steps-1)
- [**How to Use This Repository**](#how-to-use-this-repository)
- [**Contributing**](#contributing)
- [**License**](#license)
- [**Next Steps**](#next-steps)

---

## **Prerequisites**

Before getting started, ensure you have the following:

- **.NET SDK**: Install the latest .NET SDK from [here](https://dotnet.microsoft.com/download).
- **Azure Account**: Required for deploying to Azure App Services. Sign up [here](https://azure.microsoft.com/).
- **GitHub Account**: Required for using GitHub Actions. Sign up [here](https://github.com/).
- **Basic Knowledge**:
  - Familiarity with .NET Web API development.
  - Basic understanding of CI/CD concepts.
  - Knowledge of YAML syntax (used for pipeline configuration).

---

## **Repository Structure**

```
dotnet-ci-cd-examples/
├── src/
│   └── DotNetWebApi/                  # Basic .NET Web API application
│       ├── Controllers/
│       ├── Program.cs
│       ├── appsettings.json
│       └── DotNetWebApi.csproj
├── azure-pipelines/                   # Azure Pipelines example
│   └── azure-pipelines.yml            # Azure Pipelines configuration
├── github-actions/                    # GitHub Actions example
│   └── dotnet-ci-cd.yml               # GitHub Actions workflow
├── README.md                          # This file
└── .gitignore                         # Git ignore file
```

---

## **Example 1: Azure Pipelines**

This example demonstrates how to set up a CI/CD pipeline using **Azure Pipelines** to build and deploy a .NET Web API application to **Azure App Services**.

### **Steps**

1. **Create an Azure App Service**:

   - Log in to the Azure portal.
   - Create a new App Service plan and Web App.
   - Note down the Web App name and resource group for later use.

2. **Set Up Azure Pipelines**:

   - Navigate to Azure DevOps and create a new project.
   - Go to **Pipelines** > **Create Pipeline** and connect your GitHub repository.
   - Use the `azure-pipelines/azure-pipelines.yml` file provided in this repository.

3. **Pipeline Configuration**:

   - The `azure-pipelines.yml` file includes:
     - **Build**: Restores dependencies, builds the project, and runs unit tests.
     - **Deploy**: Publishes the application and deploys it to Azure App Services.

4. **Run the Pipeline**:
   - Push changes to your repository to trigger the pipeline.
   - Monitor the pipeline in Azure DevOps to ensure successful deployment.

---

## **Example 2: GitHub Actions**

This example demonstrates how to set up a CI/CD pipeline using **GitHub Actions** to build and deploy a .NET Web API application to **Azure App Services**.

### **Steps**

1. **Create an Azure App Service**:

   - Log in to the Azure portal.
   - Create a new App Service plan and Web App.
   - Note down the Web App name and resource group for later use.

2. **Set Up GitHub Secrets**:

   - Go to your GitHub repository > **Settings** > **Secrets** > **Actions**.
   - Add the following secrets:
     - `AZURE_CREDENTIALS`: JSON credentials for Azure service principal.
     - `AZURE_SUBSCRIPTION_ID`: Your Azure subscription ID.
     - `AZURE_RESOURCE_GROUP`: The resource group for your App Service.
     - `AZURE_APP_NAME`: The name of your Azure Web App.

3. **Pipeline Configuration**:

   - The `github-actions/dotnet-ci-cd.yml` file includes:
     - **Build**: Restores dependencies, builds the project, and runs unit tests.
     - **Deploy**: Publishes the application and deploys it to Azure App Services.

4. **Run the Workflow**:
   - Push changes to your repository to trigger the GitHub Actions workflow.
   - Monitor the workflow in the **Actions** tab of your GitHub repository.

---

## **License**

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## **Next Steps**

- Experiment with additional pipeline stages (e.g., integration testing, performance testing).
- Explore deploying to other cloud platforms (e.g., AWS, GCP).
- Learn about advanced DevOps practices like blue-green deployments and canary releases.

Happy coding! 🚀
