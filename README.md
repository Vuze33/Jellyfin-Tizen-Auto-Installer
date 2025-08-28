# Jellyfin Tizen Auto-Installer Script

This script provides a hardened, user-friendly method for installing the Jellyfin app on Samsung Tizen TVs **without Tizen Studio**. It fetches the latest available builds from GitHub, scans your network for compatible Samsung TVs in Developer Mode, and automates the installation process using Docker.

Huge thanks to jeppevinkel for providing the Jellyfin Tizen .wgt builds—super helpful and much appreciated! 

---

## Features

- **No Tizen Studio Required**: Installs directly using Developer Mode over the network.
- **Automatic Build Fetching**: Retrieves latest `.wgt` builds from the official releases.
- **Network Scanning**: Detects Samsung TVs on your local network (requires `nmap` for scanning).
- **Interactive & Non-interactive Modes**: Supports both user-friendly prompts and automation.
- **Caching & Validation**: Downloads and caches build files with integrity checks.
- **Docker-based Isolation**: Uses a dedicated Docker image for the install process.
- **Enhanced Logging & Retry Logic**: Improved error messages and multiple attempts for reliability.

---

## Usage

### **Prerequisites**

- **Docker** installed and usable by your user.
- **jq** (optional, for faster JSON parsing: `sudo apt install jq`).
- **nmap** (for automatic TV detection: `sudo apt install nmap`).
- **curl** (usually preinstalled).
- **Optional:** `timeout` utility for better control over command timeouts.

### **Running the Script**

1. Download the script (e.g., `installer.sh`) and make it executable:
    ```sh
    chmod +x installer.sh
    ```

2. Run the script:
    ```sh
    ./installer.sh
    ```

    - For **non-interactive automation** (selects first build and first TV found):
      ```sh
      ./installer.sh --non-interactive
      ```

3. **Follow the prompts**:
    - Select a Jellyfin build if multiple are available.
    - The script will scan your local network for Samsung TVs. If none are found, you can enter your TV's IP manually.
    - The script will attempt to install the app and provide troubleshooting steps if it fails.

---

## **Troubleshooting**

- **Developer Mode** must be enabled on your TV:  
  *Settings → General → External Device Manager → Developer Mode: ON*  
  *(You can enter any name for Developer IP; restart the TV after enabling.)*

- **Ensure your PC and TV are on the same LAN/subnet.**

- If your TV isn't found automatically, check its IP from your router or TV network settings and enter it manually.

- **Common error solutions:**
    - Grant your user permission to Docker:  
      `sudo usermod -aG docker $USER && newgrp docker`
    - If you see rate-limit errors from GitHub, wait and try again or authenticate with a personal access token.

---

## **Manual Command Example**

If the auto-installer fails, you can try running the Docker command manually:

```sh
docker run --rm --network host ghcr.io/georift/install-jellyfin-tizen:latest <TV_IP> "<PACKAGE_ID>" "<RELEASE_URL>"
