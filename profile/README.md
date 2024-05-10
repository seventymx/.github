<p align="center">
  <img src="https://github.com/seventymx/.github/blob/main/profile/were-sending-you-back-to-the-future.gif">
</p>

## TODO

### 1. **Environment Setup with Nix**

-   **Nix Flakes:** Start by setting up a Nix Flake for each of your environments—Flutter, Vue.js, and React. This will ensure that all developers are working with the same tool versions and dependencies.
-   **WSL2 and macOS:** Make sure your Nix setup is operational on both Ubuntu in WSL2 and macOS, using the same flake configurations to ensure consistency across platforms.

#### WSL2 Setup

```bash
# Install WSL2
wsl --install -d Ubuntu

# Install Nix (single-user)
sh <(curl -L https://nixos.org/nix/install) --no-daemon

# Enable Nix flakes
mkdir -p ~/.config/nix
touch ~/.config/nix/nix.conf
echo "experimental-features = nix-command flakes" >> ~/.config/nix/nix.conf

# Create workspace
sudo mkdir /wsl_workspace
sudo chmod -R 777 /wsl_workspace
cd /wsl_workspace

# Switch git credentials to store
git config --global credential.helper store

# Copy .git-credentials from Windows user profile or set up with keys for GitHub and Azure DevOps
cp ./.git-credentials ~/.git-credentials
chmod 600 ~/.git-credentials

# Set up Git config
git config --global user.email "steffen@seventy.mx"
git config --global user.name "Steffen70"

# Clone repositories or create a new one (e.g., flutter_app)
mkdir flutter_app
cd flutter_app
git init
# Add origin

# Restart WSL2
wsl --shutdown
wsl
cd /wsl_workspace/flutter_app

# Initialize Nix flakes
nix flake init

# Configure flake.nix

# Commit and push
git add *
git branch -m main
git commit -m "Initialized flake.nix"
git push --set-upstream origin main
git push
```

#### macOS Setup

-   **Nix Installation:** Install Nix on macOS (mac only supports multi-user installation).
-   **Flake Configuration:** Set up the same flake configurations as in WSL2.

Add the following to your `~/.bashrc` or `~/.zshrc`:

```bash
# Enable colors in Zsh
autoload -U colors && colors

# Add zsh prompt with colors
PROMPT='%F{green}%n@%m%f:%F{blue}%~%f$ '

# Add Visual Studio Code to PATH
export PATH="$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"

# Add environment variable for Android ADB server address
export ANDROID_ADB_SERVER_ADDRESS="172.20.2.158"
echo "ANDROID_ADB_SERVER_ADDRESS set to $ANDROID_ADB_SERVER_ADDRESS"

# Start Nix daemon
. /nix/var/nix/profiles/default/etc/profile.d/nix-daemon.sh
```

This will ensure that the Nix daemon is running and that the Visual Studio Code binary is in your path.

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
