# Dedicated Server Setup Guide for Farming Simulator 25 on AMP (Linux VPS)

Since **Farming Simulator 25 (FS25)** does not have a native Linux server release or anonymous SteamCMD download, hosting it requires utilizing Wine with a virtual display buffer (Xvfb) inside AMP, and downloading it via SteamCMD using an account that owns the game license.

This guide details how to import the custom AMP template using the official Git repository method without ever needing to log directly into your VPS via SSH.

---

## 📋 Prerequisites
1. **AMP (Cubecoders Application Management Panel)** installed on your Linux VPS.
2. A **Steam Account** that owns **Farming Simulator 25** (App ID `2300320`). GIANTS does not support anonymous downloads for this game.
3. A **dedicated server license** key if you want to play on the server at the same time as hosting it.
4. A **GitHub Account** (free) to host your configuration files.

---

## 🛠️ Step 1: Upload the Template to GitHub
CubeCoders designed AMP to seamlessly pull custom generic templates directly from GitHub. This is the official and easiest way to install a custom configuration.

1. Go to [GitHub](https://github.com/) and create a new **Public** repository (e.g., name it `FS25-AMP-Template`).
2. Upload the 4 configuration files from your Documents folder into the root of this new GitHub repository:
   - `farming-simulator-25.kvp`
   - `farming-simulator-25config.json`
   - `farming-simulator-25metaconfig.json`
   - `farming-simulator-25ports.json`
3. Make sure the files are on your `main` or `master` branch.

---

## 🔄 Step 2: Import the Template into AMP
Now, we will tell AMP to download the template straight from your GitHub repository.

1. Log into your **AMP Controller Web Interface**.
2. In the left sidebar, navigate to **Configuration** -> **Instance Deployment**.
3. Scroll down to the **Configuration Repositories** section.
4. Click **Add** to add a new repository.
5. Enter your repository details in the format `Username/RepositoryName:Branch`.
   *(In your case, you will type `robertbiv/AMPtemplates:master`)*
6. Click the **Fetch** button.
7. **Refresh your browser page**. AMP has now securely downloaded and registered your FS25 template!

---

## 🚀 Step 3: Creating and Configuring the Instance
1. In AMP, click **Create Instance**.
2. Select **Generic Module** (often labeled as generic/custom applications).
3. Under the **Generic Module Settings**, open the dropdown menu and select **Farming Simulator 25**.
4. Name the instance (e.g., `FS25Server`) and click **Create Instance**.
5. Once created, click **Manage** to open the instance control panel.

---

## 📥 Step 4: Downloading the Game Files via SteamCMD
Because FS25 is a paid game with no anonymous download:
1. Navigate to **Instance Settings** -> **SteamCMD** in AMP.
2. Change the update login method to use **Steam Login Credentials** (disable Anonymous login).
3. Input your Steam account Username and Password (ensure this account owns Farming Simulator 25).
4. Click **Save Settings**.
5. Click **Update** in the instance dashboard. AMP will run SteamCMD, log in with your credentials (it may prompt you for your Steam Guard 2FA code in the console log), and download the full game.

---

## 🔑 Step 5: Web Panel Login & License Activation
When SteamCMD completes the download, start the instance. AMP will spin up a Wine container running `dedicatedServer.exe` under Xvfb.

1. Once the instance is running, open a web browser and navigate to:
   `http://<your-vps-ip>:<WebAdminPort>` (Check your AMP panel ports for the exact `Web Admin Port` allocated, usually 8080).
2. Log in using the Web Admin credentials configured in your AMP UI (default: Username `admin`, Password `admin12345`).
3. You will be greeted by the **GIANTS Software license key screen**.
4. Enter your dedicated server product key to activate the game.
5. Once activated, configure your server options (map, savegame slot, passwords, mod list, etc.) in the GIANTS web UI and click **Start** to run the game simulation.

> **IMPORTANT**: In the GIANTS Web Panel, ensure the game port setting matches the **Game Connection Port** assigned to your instance in AMP (default is `10823`).

---

> Since the server runs in a persistent Wine environment (`WINEPREFIX`), all license activation states and mod downloads will remain intact across restarts and updates!
