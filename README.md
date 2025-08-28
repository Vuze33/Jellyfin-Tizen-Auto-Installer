# Jellyfin Tizen Auto-Installer

Easily install the latest Jellyfin app builds on your Samsung Tizen TV **without Tizen Studio**—using only Docker and your local network.  
This script brings together the best open-source tools for a seamless, repeatable, and interactive (or fully automated) installation process.

---

## Features

- **No Tizen Studio Needed:** Uses the TV's Developer Mode—no Samsung account or Tizen IDE required.
- **Automatic Build Fetch:** Grabs the latest `.wgt` builds from [jeppevinkel/jellyfin-tizen-builds](https://github.com/jeppevinkel/jellyfin-tizen-builds).
- **Network Scan:** Searches your local network for compatible Samsung TVs (requires `nmap`).
- **Interactive & Automated:** Use interactively (with prompts) or non-interactively for scripting/CI.
- **Caching & Validation:** Downloads and validates builds, caching for future use.
- **Docker Powered:** Leverages a prebuilt Docker image with the Tizen SDK for environment isolation.
- **Robust Logging & Retries:** Enhanced error output and automatic retries for reliability.

---

## Prerequisites

- **Docker** (must be installed and your user must have permission to run it)
- **curl** (for HTTP requests, usually pre-installed)
- **nmap** (for network scanning: `sudo apt install nmap` or equivalent)
- **jq** (optional, speeds up JSON parsing: `sudo apt install jq`)
- **timeout** (optional, for reliable timeouts: `sudo apt install coreutils` or `brew install coreutils`)

---

## Quick Start

1. **Download the Script**

   ```
   wget https://github.com/Vuze33/Jellyfin-Tizen-Auto-Installer/releases/download/Release/install-jellyfin.sh
   ```

2. **Make the script executable**

   ```
   chmod +x install-jellyfin.sh
   ```

3. **Run the Installer**

   ```
   ./install-jellyfin.sh
   ```

4. **Follow Prompts**

   - Choose your preferred Jellyfin build (if multiple available).
   - Select your Samsung TV (auto-detected or enter IP manually).
   - Installation progress and troubleshooting tips are shown as needed.

---

## How It Works

1. **Fetches latest builds** from [jeppevinkel/jellyfin-tizen-builds](https://github.com/jeppevinkel/jellyfin-tizen-builds) via the GitHub API.
2. **Scans your local network** for Samsung TVs in Developer Mode (port 26101).
3. **Downloads and validates** the selected `.wgt` build (caches for reuse).
4. **Runs a Docker container** ([ghcr.io/georift/install-jellyfin-tizen](https://github.com/Georift/install-jellyfin-tizen)) to push the app to your TV.
5. **Logs output and errors**, retries if needed, and provides detailed troubleshooting if installation fails.

---

## Troubleshooting

- ** 📺 Enable Developer Mode on Samsung TV

To install or sideload apps (like Jellyfin) on Samsung Tizen TVs, you first need to enable **Developer Mode**.

1. On your Samsung remote, press the **Home** button.  
2. Navigate to **Apps** and open it.  
3. While on the **Apps** screen, press the following number sequence on your remote:  1 2 3 4 5
4. A hidden Developer Mode menu will appear. Set Developer Mode → ON.

- **Find your TV's IP:**
  - Check your router’s device list, or on the TV: *Settings → Network → Network Status*.
- **Network Tips:**
  - Ensure your PC and TV are on the same LAN/subnet.
  - Temporarily disable firewalls if discovery fails.
- **Docker Permissions:**
  - If you see Docker permission errors:  
    `sudo usermod -aG docker $USER && newgrp docker`
- **Manual Install:**
  - Use the provided Docker command in the error output for manual troubleshooting.

---

## Manual Docker Command Example

If you need to run the install manually (e.g., for custom builds):

```sh
docker run --rm --network host ghcr.io/georift/install-jellyfin-tizen:latest <TV_IP> "<PACKAGE_ID>" "<RELEASE_URL>"
```
Replace `<TV_IP>`, `<PACKAGE_ID>`, and `<RELEASE_URL>` with values shown in the script output.

---

## Caching & Files

- Build files are cached in: `~/.jellyfin-tizen-cache/`
- Temporary logs: `/tmp/jellyfin_install.log.<pid>`

---

## Credits

This is possible thanks to these projects and contributors. This repository is simply a wrapper to make their work more accessible.

- [jellyfin-tizen](https://github.com/jellyfin/jellyfin-tizen)  
  The official Jellyfin client for Samsung Tizen TVs.

- [jeppevinkel/jellyfin-tizen-builds](https://github.com/jeppevinkel/jellyfin-tizen-builds)  
  Community-provided and bleeding-edge Jellyfin builds.

- [vitalets/docker-tizen-webos-sdk](https://github.com/vitalets/docker-tizen-webos-sdk)  
  Docker image with the Tizen SDK, simplifying automation.

- [Georift/install-jellyfin-tizen](https://github.com/Georift/install-jellyfin-tizen)  
  This repo (@Georift), providing the Docker wrapper and install script.

---

### Special thanks

To all maintainers and contributors to the above projects.  
If this helped you, please ⭐️ their repositories and consider supporting open-source development!
