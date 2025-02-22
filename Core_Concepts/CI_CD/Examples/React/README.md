### **README: CI/CD Pipeline Examples for Deploying a Basic React Application**

This repository provides two practical examples of setting up CI/CD pipelines to deploy a basic **React** application:

1. **Azure Pipelines**: Automating build and deployment using Azure DevOps.
2. **GitHub Actions**: Automating build and deployment using GitHub Actions.

These examples are designed to help you understand how to implement Continuous Integration and Continuous Deployment (CI/CD) for React applications in two popular platforms. Additionally, this guide includes information on how the React frontend can dynamically connect to its backend API in different environments (e.g., dev, UAT, prod).

---

## **Table of Contents**

- [**Table of Contents**](#table-of-contents)
- [**Prerequisites**](#prerequisites)
- [**Repository Structure**](#repository-structure)
- [**Connecting Frontend to Backend in Different Environments**](#connecting-frontend-to-backend-in-different-environments)
  - [**1. Environment Variables**](#1-environment-variables)
  - [**2. Access Environment Variables in React**](#2-access-environment-variables-in-react)
  - [**3. Configure CI/CD Pipelines**](#3-configure-cicd-pipelines)
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

- **Node.js**: Install the latest LTS version from [here](https://nodejs.org/).
- **Azure Account**: Required for deploying to Azure Static Web Apps. Sign up [here](https://azure.microsoft.com/).
- **GitHub Account**: Required for using GitHub Actions. Sign up [here](https://github.com/).
- **Basic Knowledge**:
  - Familiarity with React development.
  - Basic understanding of CI/CD concepts.
  - Knowledge of YAML syntax (used for pipeline configuration).

---

## **Repository Structure**

```
react-ci-cd-examples/
├── src/
│   └── react-app/                  # Basic React application
│       ├── public/
│       ├── src/
│       │   ├── App.js
│       │   ├── index.js
│       │   └── api.js              # File for API configuration
│       ├── package.json
│       └── .env                    # Environment variables
├── azure-pipelines/                # Azure Pipelines example
│   └── azure-pipelines.yml         # Azure Pipelines configuration
├── github-actions/                 # GitHub Actions example
│   └── react-ci-cd.yml             # GitHub Actions workflow
├── README.md                       # This file
└── .gitignore                      # Git ignore file
```

---

## **Connecting Frontend to Backend in Different Environments**

In a real-world application, the React frontend needs to connect to a backend API (e.g., a .NET Web API). The backend API URL will differ across environments (dev, UAT, prod). Here’s how to handle this:

### **1. Environment Variables**

- Use environment variables to store the backend API URL for each environment.
- Create `.env` files for each environment:
  - `.env.development`: For the `develop` branch.
  - `.env.uat`: For the `release` branch.
  - `.env.production`: For the `main` branch.

Example `.env` file:

```env
REACT_APP_API_URL=https://api-dev.example.com
```

### **2. Access Environment Variables in React**

- Use `process.env.REACT_APP_API_URL` in your React code to access the backend API URL.
- Example in `src/api.js`:

  ```javascript
  const apiUrl = process.env.REACT_APP_API_URL;

  export const fetchData = async () => {
    const response = await fetch(`${apiUrl}/data`);
    return response.json();
  };
  ```

### **3. Configure CI/CD Pipelines**

- Inject the correct environment variables during the build process based on the branch:
  - `develop` → Use `.env.development`.
  - `release` → Use `.env.uat`.
  - `main` → Use `.env.production`.

---

## **Example 1: Azure Pipelines**

This example demonstrates how to set up a CI/CD pipeline using **Azure Pipelines** to build and deploy a React application to **Azure Static Web Apps**.

### **Steps**

1. **Create an Azure Static Web App**:

   - Log in to the Azure portal.
   - Create a new Static Web App for each environment (dev, UAT, prod).
   - Note down the Static Web App names and resource groups for later use.

2. **Set Up Azure Pipelines**:

   - Navigate to Azure DevOps and create a new project.
   - Go to **Pipelines** > **Create Pipeline** and connect your GitHub repository.
   - Use the `azure-pipelines/azure-pipelines.yml` file provided in this repository.

3. **Pipeline Configuration**:

   - The `azure-pipelines.yml` file includes:
     - **Build**: Installs dependencies, builds the React app, and injects environment variables.
     - **Deploy**: Deploys the app to the appropriate Azure Static Web App based on the branch.

4. **Run the Pipeline**:
   - Push changes to your repository to trigger the pipeline.
   - Monitor the pipeline in Azure DevOps to ensure successful deployment.

---

## **Example 2: GitHub Actions**

This example demonstrates how to set up a CI/CD pipeline using **GitHub Actions** to build and deploy a React application to **Azure Static Web Apps**.

### **Steps**

1. **Create an Azure Static Web App**:

   - Log in to the Azure portal.
   - Create a new Static Web App for each environment (dev, UAT, prod).
   - Note down the Static Web App names and resource groups for later use.

2. **Set Up GitHub Secrets**:

   - Go to your GitHub repository > **Settings** > **Secrets and variables** > **Actions**.
   - Add the following secrets:
     - `AZURE_CREDENTIALS`: JSON credentials for Azure service principal.
     - `AZURE_SUBSCRIPTION_ID`: Your Azure subscription ID.
     - `AZURE_RESOURCE_GROUP`: The resource group for your Static Web App.
     - `AZURE_APP_NAME_PREFIX`: The prefix for your Static Web App names (e.g., `ReactApp`).

3. **Pipeline Configuration**:

   - The `github-actions/react-ci-cd.yml` file includes:
     - **Build**: Installs dependencies, builds the React app, and injects environment variables.
     - **Deploy**: Deploys the app to the appropriate Azure Static Web App based on the branch.

4. **Run the Workflow**:
   - Push changes to your repository to trigger the GitHub Actions workflow.
   - Monitor the workflow in the **Actions** tab of your GitHub repository.

---

## **How to Use This Repository**

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/react-ci-cd-examples.git
   cd react-ci-cd-examples
   ```
2. Explore the `src/react-app` folder to understand the basic React application.
3. Follow the steps in [Example 1](#example-1-azure-pipelines) or [Example 2](#example-2-github-actions) to set up your CI/CD pipeline.
4. Modify the pipeline configurations as needed for your specific use case.

---

## **Contributing**

Contributions are welcome! If you have suggestions, improvements, or additional examples, feel free to:

1. Fork this repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m 'Add some feature'`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

---

## **License**

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## **Next Steps**

- Experiment with additional pipeline stages (e.g., integration testing, performance testing).
- Explore deploying to other cloud platforms (e.g., AWS, GCP).
- Learn about advanced DevOps practices like blue-green deployments and canary releases.

Happy coding! 🚀
