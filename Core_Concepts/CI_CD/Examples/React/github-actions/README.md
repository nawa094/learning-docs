Here is an example of a **GitHub Actions workflow file** (`react-ci-cd.yml`) for deploying a **React** application to **Azure Static Web Apps**. This pipeline follows **Git Flow**, where:

- The `main` branch is deployed to **Production**.
- The `release` branch is deployed to **UAT** (User Acceptance Testing).
- The `develop` branch is deployed to **Dev**.

The pipeline includes steps to:

1. **Build** the React application.
2. **Inject environment variables** for the backend API URL.
3. **Deploy** to the appropriate Azure Static Web App instance based on the branch.

---

### **Explanation of the Workflow**

#### **1. Trigger**

- The workflow triggers on changes to the `main`, `release`, and `develop` branches.

#### **2. Environment Variables**

- `AZURE_STATIC_WEB_APPS_RESOURCE_GROUP`: Your Azure resource group name.
- `AZURE_STATIC_WEB_APPS_NAME`: Dynamically sets the Azure Static Web App name based on the branch:
  - `main` → `your-app-name-prefix-prod`
  - `release` → `your-app-name-prefix-uat`
  - `develop` → `your-app-name-prefix-dev`
- `NODE_VERSION`: The Node.js version to use.
- `BUILD_CONFIGURATION`: Specifies the build configuration (e.g., `production`).

#### **3. Jobs**

- **build-and-deploy**:
  - Checks out the code.
  - Sets up Node.js.
  - Injects environment variables for the backend API URL based on the branch.
  - Installs dependencies and builds the React app.
  - Deploys the app to the appropriate Azure Static Web App based on the branch.

---

### **Prerequisites**

1. **Azure Static Web Apps**:
   - Create three Azure Static Web Apps:
     - `your-app-name-prefix-prod` (for `main` branch).
     - `your-app-name-prefix-uat` (for `release` branch).
     - `your-app-name-prefix-dev` (for `develop` branch).
2. **GitHub Secrets**:
   - Go to your GitHub repository > **Settings** > **Secrets and variables** > **Actions**.
   - Add the following secrets:
     - `AZURE_STATIC_WEB_APPS_TOKEN`: Deployment token for your Azure Static Web App.
     - `AZURE_RESOURCE_GROUP`: The resource group for your Static Web App.
     - `AZURE_APP_NAME_PREFIX`: The prefix for your Static Web App names (e.g., `ReactApp`).

---

### **How to Use**

1. Replace placeholders (e.g., `your-app-name-prefix`, `your-resource-group-name`) with your actual Azure details.
2. Push the `react-ci-cd.yml` file to your repository under the `.github/workflows/` directory.
3. The workflow will automatically trigger on changes to the `main`, `release`, or `develop` branches.
4. Monitor the workflow in the **Actions** tab of your GitHub repository.

---

### **Example GitHub Secrets**

Here’s an example of how to set up the `AZURE_STATIC_WEB_APPS_TOKEN` secret:

1. Go to your Azure Static Web App in the Azure portal.
2. Navigate to **Deployment Tokens** and generate a new token.
3. Copy the token and add it as the `AZURE_STATIC_WEB_APPS_TOKEN` secret in GitHub.

---

### **Connecting Frontend to Backend**

The workflow dynamically sets the `REACT_APP_API_URL` environment variable based on the branch:

- `main` → `https://api-prod.example.com`
- `release` → `https://api-uat.example.com`
- `develop` → `https://api-dev.example.com`

This ensures the React app connects to the correct backend API in each environment.

---

This workflow provides a robust foundation for implementing Git Flow with GitHub Actions for a React application. Let me know if you need further assistance! 🚀
