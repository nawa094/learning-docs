Here is an example of an **Azure Pipelines YAML file** (`azure-pipelines.yml`) for deploying a **React** application to **Azure Static Web Apps**. This pipeline follows **Git Flow**, where:

- The `main` branch is deployed to **Production**.
- The `release` branch is deployed to **UAT** (User Acceptance Testing).
- The `develop` branch is deployed to **Dev**.

The pipeline includes steps to:

1. **Build** the React application.
2. **Inject environment variables** for the backend API URL.
3. **Deploy** to the appropriate Azure Static Web App instance based on the branch.

---

### **Explanation of the Pipeline**

#### **1. Trigger**

- The pipeline triggers on changes to the `main`, `release`, and `develop` branches.

#### **2. Variables**

- `appNamePrefix`: Prefix for the Azure Static Web App name.
- `resourceGroup`: Your Azure resource group name.
- `azureSubscription`: Your Azure subscription name.

#### **3. Stages**

- **Build Stage**:
  - Sets up Node.js.
  - Checks out the code.
  - Injects environment variables for the backend API URL based on the branch.
  - Installs dependencies and builds the React app.
  - Publishes the build artifacts.
- **Deploy Stage**:
  - Downloads the build artifacts.
  - Deploys the app to the appropriate Azure Static Web App based on the branch.

#### **4. Parameters**

- `appName`: Dynamically sets the Azure Static Web App name based on the branch:
  - `main` → `ReactApp-prod`
  - `release` → `ReactApp-uat`
  - `develop` → `ReactApp-dev`

#### **5. Deployment**

- The `AzureStaticWebApp` task deploys the React app to Azure Static Web Apps.

---

### **Prerequisites**

1. **Azure Static Web Apps**:
   - Create three Azure Static Web Apps:
     - `ReactApp-prod` (for `main` branch).
     - `ReactApp-uat` (for `release` branch).
     - `ReactApp-dev` (for `develop` branch).
2. **Azure Service Connection**:
   - Set up an Azure service connection in Azure DevOps:
     - Go to **Project Settings** > **Service Connections** > **New Service Connection** > **Azure Resource Manager**.
     - Provide your Azure subscription details.
3. **Azure Static Web Apps Token**:
   - Generate a deployment token for each Static Web App and store it as a secret in Azure DevOps.

---

### **How to Use**

1. Replace placeholders (e.g., `your-azure-subscription-name`, `your-resource-group-name`) with your actual Azure details.
2. Push the `azure-pipelines.yml` file to your repository.
3. The pipeline will automatically trigger on changes to the `main`, `release`, or `develop` branches.
4. Monitor the pipeline in Azure DevOps to ensure successful builds and deployments.

---

This pipeline provides a robust foundation for implementing Git Flow with Azure Pipelines for a React application. Let me know if you need further assistance! 🚀
