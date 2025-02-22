This is an example of a **GitHub Actions workflow file** (`dotnet-ci-cd.yml`) that implements a CI/CD pipeline for a .NET Web API application. This pipeline follows **Git Flow**, where:

- The `main` branch is deployed to **Production**.
- The `release` branch is deployed to **UAT** (User Acceptance Testing).
- The `develop` branch is deployed to **Dev**.

The pipeline includes steps to:

1. **Build** the .NET Web API application.
2. **Run tests** (if any).
3. **Deploy** to the appropriate Azure Web App instance based on the branch.

---

### **Explanation of the Workflow**

#### **1. Trigger**

- The workflow triggers on changes to the `main`, `release`, and `develop` branches.

#### **2. Environment Variables**

- `AZURE_WEBAPP_NAME`: Dynamically sets the Azure Web App name based on the branch:
  - `main` → `your-app-name-prefix-prod`
  - `release` → `your-app-name-prefix-uat`
  - `develop` → `your-app-name-prefix-dev`
- `AZURE_RESOURCE_GROUP`: Your Azure resource group name.
- `AZURE_SUBSCRIPTION_ID`: Your Azure subscription ID.
- `DOTNET_VERSION`: The .NET SDK version to use.
- `BUILD_CONFIGURATION`: Specifies the build configuration (e.g., `Release`).

#### **3. Jobs**

- **build-and-test**:
  - Checks out the code.
  - Sets up the .NET SDK.
  - Restores dependencies, builds the project, and runs tests.
- **deploy**:
  - Publishes the .NET project to a folder.
  - Logs in to Azure using the provided credentials.
  - Deploys the published artifacts to the appropriate Azure Web App based on the branch.

---

### **Prerequisites**

1. **Azure Web Apps**:
   - Create three Azure Web Apps:
     - `your-app-name-prefix-prod` (for `main` branch).
     - `your-app-name-prefix-uat` (for `release` branch).
     - `your-app-name-prefix-dev` (for `develop` branch).
2. **GitHub Secrets**:
   - Go to your GitHub repository > **Settings** > **Secrets and variables** > **Actions**.
   - Add the following secrets:
     - `AZURE_CREDENTIALS`: JSON credentials for Azure service principal.
     - `AZURE_SUBSCRIPTION_ID`: Your Azure subscription ID.
     - `AZURE_RESOURCE_GROUP`: The resource group for your App Service.
     - `AZURE_APP_NAME_PREFIX`: The prefix for your Azure Web App names (e.g., `DotNetWebApi`).
     - `AZURE_PUBLISH_PROFILE`: The publish profile for your Azure Web App.

---

### **How to Use**

1. Replace placeholders (e.g., `your-app-name-prefix`, `your-resource-group-name`) with your actual Azure details.
2. Push the `dotnet-ci-cd.yml` file to your repository under the `.github/workflows/` directory.
3. The workflow will automatically trigger on changes to the `main`, `release`, or `develop` branches.
4. Monitor the workflow in the **Actions** tab of your GitHub repository.

---

### **Example GitHub Secrets**

Here’s an example of how to set up the `AZURE_CREDENTIALS` secret:

1. Create a service principal in Azure:
   ```bash
   az ad sp create-for-rbac --name "github-actions-sp" --role contributor \
   --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
   --sdk-auth
   ```
2. Copy the JSON output and add it as the `AZURE_CREDENTIALS` secret in GitHub.

---

This workflow provides a robust foundation for implementing Git Flow with GitHub Actions. You can extend it further by adding steps for integration testing, performance testing, or approvals for production deployments. Let me know if you need further assistance! 🚀
