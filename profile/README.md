<p align="center">
  <img src="https://github.com/seventymx/.github/blob/main/profile/were-sending-you-back-to-the-future.gif">
</p>

## TODO

### 1. **Environment Setup with Nix**

-   **Nix Flakes:** Start by setting up a Nix Flake for each of your environments—Flutter, Vue.js, and React. This will ensure that all developers are working with the same tool versions and dependencies.
-   **WSL2 and macOS:** Make sure your Nix setup is operational on both Ubuntu in WSL2 and macOS, using the same flake configurations to ensure consistency across platforms.

### 2. **Docker Integration**

-   **Microservices Build Images:** For each microservice, create Docker images that contain the Nix package manager to ensure the correct build tools and versions are used. These images will be used to build native compliant applications or additional Docker images for hosting.
-   **Hosting Platforms:** Prepare configurations for deployment on AWS/Azure or on-premise solutions, ensuring that your Docker images are optimized for these environments.

### 3. **Development of Microservices**

-   **Service 1: Custom Identity Provider** using gRPC for communication, which integrates with your existing user database and allows for additional identity providers like Azure AD.
-   **Service 2: Authorization Service** that integrates roles and claims into tokens, leveraging your legacy database and mapping AD users to Swisspension roles.
-   **Service 3: File Archiver** for a simple file upload and storage service.
-   **Service 4: Legacy-Compatible Service** that uses COM references, specifically for Microsoft Word, to demonstrate the ability to handle legacy systems within your modernized infrastructure.

### 4. **CI/CD Integration**

-   **Automated Builds:** Set up CI/CD pipelines for Services 1-3 and the React project, using GitHub Actions or another CI/CD tool to automate builds and deployments upon code commits.
-   **Manual Builds:** Develop build scripts in PowerShell Core for the Flutter app and the legacy archiver, allowing these components to be built manually but streamlined with modern scripting.

### 5. **Deployment and Distribution**

-   **Automated Deployment:** Use automated scripts to deploy built images of Services 1-3 and the React application to Azure.
-   **Manual Deployment:** For the Flutter app, create deployment processes to the Google Play Store and Apple App Store. For the legacy archiver, deploy using Azure CLI scripted in PowerShell.

### 6. **Repository Management**

-   **GitHub Structure:** Consider using GitHub's multiple repositories for each service and front-end application. You can link these repositories using GitHub's project boards or a meta repository with submodules linking back to each service repo.
-   **Documentation:** Ensure each repository is well-documented, particularly the legacy archiver, detailing the steps for environment setup and build processes.

### 7. **Demonstration and Evaluation**

-   **Showcasing the System:** Prepare a demo or a presentation to showcase the functionality of your system, the ease of development setup, and the efficiency of your CI/CD pipelines.
