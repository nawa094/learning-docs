### **Containerization and Deployment of a React Application Built with Vite**

This repository provides practical examples of **containerizing a React application built with Vite** and deploying it to a **container registry** using CI/CD pipelines. The examples cover:

1. **Azure Pipelines**: Containerizing and deploying to Azure Container Registry (ACR).
2. **GitHub Actions**: Containerizing and deploying to Docker Hub or GitHub Container Registry (GHCR).

Both pipelines reference the same **Dockerfile** to ensure consistency in the containerization process.

---

## **Table of Contents**

- [**Table of Contents**](#table-of-contents)
- [**Prerequisites**](#prerequisites)
- [**Potential Repository Structure**](#potential-repository-structure)
- [**Example 1: Azure Pipelines**](#example-1-azure-pipelines)
  - [**azure-pipelines.yml**](#azure-pipelinesyml)
- [**Example 2: GitHub Actions**](#example-2-github-actions)
  - [**github-actions/react-vite-ci-cd.yml**](#github-actionsreact-vite-ci-cdyml)
- [**License**](#license)
- [**Next Steps**](#next-steps)

---

## **Prerequisites**

Before getting started, ensure you have the following:

- **Node.js**: Install the latest LTS version from [here](https://nodejs.org/).
- **Docker**: Install Docker Desktop from [here](https://www.docker.com/products/docker-desktop).
- **Azure Account**: Required for Azure Container Registry. Sign up [here](https://azure.microsoft.com/).
- **GitHub Account**: Required for GitHub Actions and GitHub Container Registry. Sign up [here](https://github.com/).
- **Basic Knowledge**:
  - Familiarity with React and Vite development.
  - Basic understanding of Docker and containerization.
  - Knowledge of YAML syntax (used for pipeline configuration).

---

## **Potential Repository Structure**

```
react-vite-container-examples/
├── src/
│   └── react-app/                  # Basic React application built with Vite
│       ├── public/
│       ├── src/
│       │   ├── App.jsx
│       │   ├── main.jsx
│       │   └── api.js              # File for API configuration
│       ├── package.json
│       ├── vite.config.js
│       └── .env                    # Environment variables
├── docker/                         # Docker-related files
│   └── Dockerfile                  # Dockerfile for the React application
├── azure-pipelines/                # Azure Pipelines example
│   └── azure-pipelines.yml         # Azure Pipelines configuration
├── github-actions/                 # GitHub Actions example
│   └── react-vite-ci-cd.yml        # GitHub Actions workflow
├── README.md                       # This file
└── .gitignore                      # Git ignore file
```

---

## **Example 1: Azure Pipelines**

This example demonstrates how to set up a CI/CD pipeline using **Azure Pipelines** to:

1. **Build** the React application.
2. **Containerize** it using the shared Dockerfile.
3. **Push** the container image to **Azure Container Registry (ACR)**.

### **azure-pipelines.yml**

```yaml
trigger:
  branches:
    include:
      - main
      - release
      - develop

variables:
  acrName: 'your-acr-name' # Replace with your Azure Container Registry name
  imageName: 'react-vite-app'
  tag: '${{ variables['Build.SourceBranchName'] }'

stages:
  - stage: Build
    displayName: 'Build and Test'
    jobs:
      - job: Build
        displayName: 'Build Job'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - task: UseNode@1
            inputs:
              version: '16.x' # Replace with your Node.js version

          - name: Checkout code
            uses: actions/checkout@v3

          - script: |
              npm install
              npm run build
            displayName: 'Install Dependencies and Build React App'

  - stage: Containerize
    displayName: 'Containerize and Push'
    dependsOn: Build
    condition: succeeded()
    jobs:
      - job: Docker
        displayName: 'Docker Job'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - task: Docker@2
            displayName: 'Build Docker Image'
            inputs:
              containerRegistry: '$(acrName)'
              repository: '$(imageName)'
              command: 'buildAndPush'
              Dockerfile: 'docker/Dockerfile'
              tags: '$(tag)'

          - script: |
              echo "Docker image pushed to $(acrName).azurecr.io/$(imageName):$(tag)"
            displayName: 'Log Docker Image Details'
```

---

## **Example 2: GitHub Actions**

This example demonstrates how to set up a CI/CD pipeline using **GitHub Actions** to:

1. **Build** the React application.
2. **Containerize** it using the shared Dockerfile.
3. **Push** the container image to **Docker Hub** or **GitHub Container Registry (GHCR)**.

### **github-actions/react-vite-ci-cd.yml**

```yaml
name: React Vite CI/CD Pipeline

on:
  push:
    branches:
      - main
      - release
      - develop

env:
  IMAGE_NAME: "react-vite-app"
  REGISTRY: "docker.io" # Use 'ghcr.io' for GitHub Container Registry
  REPO: "${{ secrets.DOCKER_HUB_USERNAME }}/${{ env.IMAGE_NAME }}" # Replace with your Docker Hub or GHCR repo

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "16.x" # Replace with your Node.js version

      - name: Install dependencies
        run: npm install

      - name: Build React app
        run: npm run build

  containerize-and-push:
    runs-on: ubuntu-latest
    needs: build-and-test
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v4
        with:
          context: .
          file: docker/Dockerfile
          push: true
          tags: |
            ${{ env.REGISTRY }}/${{ env.REPO }}:latest
            ${{ env.REGISTRY }}/${{ env.REPO }}:${{ github.ref_name }}
```

---

## **License**

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## **Next Steps**

- Experiment with multi-stage Docker builds to optimize image size.
- Explore deploying containerized applications to Kubernetes.
- Learn about advanced container security practices.

Happy containerizing! 🐳
