<p align="center">
  <img src="https://github.com/seventymx/.github/blob/main/profile/were-sending-you-back-to-the-future.gif">
</p>

<div align="center" style="display: flex; justify-content: center; align-items: center;">
  <a href="https://github.com/seventymx/.github/blob/main/LICENSE"><img src="https://img.shields.io/github/license/seventymx/.github?style=for-the-badge&color=indianred" alt=""></a>
</div>

## TODO

### 1. **Environment Setup with Nix**

-   **Nix Flakes:** Start by setting up a Nix Flake for each of your environments—Flutter, Vue.js, and React. This will ensure that all developers are working with the same tool versions and dependencies.
-   **WSL2 and macOS:** Make sure your Nix setup is operational on both Ubuntu in WSL2 and macOS, using the same flake configurations to ensure consistency across platforms.

#### Nix Setup

```bash
# Install Nix (single-user)
sh <(curl -L https://nixos.org/nix/install) --no-daemon

# Enable Nix flakes
mkdir -p ~/.config/nix
touch ~/.config/nix/nix.conf
echo "experimental-features = nix-command flakes" >> ~/.config/nix/nix.conf

# Add github credentials to nix for authenticated access
# nano ~/.config/nix/nix.conf
code ~/.config/nix/nix.conf

# Add the following line to the nix.conf file
access-tokens = github.com=$your_github_token

# Now you can run nix commands like nix flake update, nix develop, nix build, etc. even if the referenced repository is private (unauthenticated access will also have a lower rate limit)
```

You can also add credentials for other Platforms [Nix Manual](https://nix.dev/manual/nix/2.23/command-ref/conf-file.html).

#### WSL2 Setup

```bash
# Install WSL2
wsl --install -d Ubuntu

# Update and upgrade Ubuntu
sudo apt update
sudo apt upgrade -y

# Upgrade to 23.04 (Jammy Jellyfish - LTS)
sudo sed -i 's/jammy/lunar/g' /etc/apt/sources.list

# Or upgrade to 23.10 (Manticore)
sudo sed -i 's/jammy/mantic/g' /etc/apt/sources.list

sudo apt update
sudo apt dist-upgrade -y

# Clean up
sudo apt autoremove -y
sudo apt clean

# Restart WSL2
wsl --shutdown

# Open Ubuntu back up
wt -p "Ubuntu"

# Check versions - glibc should be 2.38 or higher (older versions cause issues with unstable.git)
lsb_release -a
ldd --version

# Install Nix (checkout section above)

# Create workspace
sudo mkdir /wsl_workspace
sudo chmod -R 777 /wsl_workspace
cd /wsl_workspace

# Set up Git config (you can also copy an existing .gitconfig to your home directory)
git config --global user.email "steffen@seventy.mx"
git config --global user.name "Steffen70"

# Switch git credentials to store
git config --global credential.helper store

# Copy .git-credentials from Windows user profile or set up with keys for GitHub and Azure DevOps
cp ./.git-credentials ~/.git-credentials
chmod 600 ~/.git-credentials

# Copy .gitconfig and .git-credentials to remote host (linx sftp tool required)
sftp $user@$remote_host
# sftp -P 2222 $user@localhost
lcd ~
put .git-credentials .git-credentials
put .gitconfig .gitconfig
exit

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

#### Create a new Flutter project

```bash
# Copy the flake.nix of an existing project (needs to be a public repository)
nix flake init --template github:seventymx/$repo_name

# Copy from private repository (needs to be authenticated)
cp ../spi_mobile/flake.nix .

# Enter environment
nix develop

# Initialize Flutter project
flutter create --platforms=android,ios .; rm -rf test

# Git init and first commit
git init; git add .; git commit -m "Created new empty Flutter project."

# Add the fastlane repository (seventymx/fastlane_ios_automation) as a submodule to ./ios/fastlane
git submodule add https://github.com/seventymx/fastlane_ios_automation.git ios/fastlane
git submodule update --init --recursive

# You also need to override some Flutter template files to ensure the correct signing and deployment configurations
cp -r ../spi_mobile/android/app/build.gradle android/app/build.gradle
git commit -am "Added fastlane submodule and updated build.gradle."

# Don't forget the ../flutter_secrets repository in your workspace for secrets management

# Copy additional nice-to-have files from your existing project
# .gitignore, .prettierrc.json, README.md, etc.
cp -r ../spi_mobile/.gitignore .; cp -r ../spi_mobile/.prettierrc.json .; cp -r ../spi_mobile/README.md .
git add .; git commit -m "Added additional files from existing project."
```

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
