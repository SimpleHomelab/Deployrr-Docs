# Waiting for Container to Start

When you have Docker pre-installed on your system (either from snap packages or OS repositories) before using Deployrr, you might encounter issues with container startup. This guide will help you resolve these problems and set up Docker correctly with Deployrr.

## Common Symptoms and the Cause

- Containers getting stuck at "Waiting for [container] to start"
- Docker commands showing "unknown flag: --profile" errors
- Older versions of Docker and Docker Compose (docker-compose instead of the new docker compose) being used

This issue occurs because the Docker version installed from snap or OS repositories is usually older. 

## Solution

Follow these steps to resolve the issue:

1. **Remove Existing Docker Installation**

   If installed via snap:
   ```bash
   sudo snap remove docker
   ```

   If installed via apt:
   ```bash
   sudo apt remove docker docker-engine docker.io containerd runc
   sudo apt autoremove
   ```

2. **Clean Up Docker Directories**
   ```bash
   sudo rm -rf /var/lib/docker
   sudo rm -rf ~/docker    # Remove Deployrr's Docker folder
   ```

3. **Reset Deployrr**
   - Open Deployrr->Settings
   - Run "Reset" option
   - This ensures Deployrr starts fresh with prerequisites

4. **Restart with Deployrr**
   - Let Deployrr handle the Docker installation
   - Follow the standard setup process
   - Deployrr will install the latest compatible Docker version from official Docker repositories.

## Important Notes

- Always let Deployrr manage Docker installation
- Don't install Docker manually before using Deployrr
- If you need to start over, always perform both the Docker cleanup and Deployrr reset
- Backup any important container data before performing these steps

## Sources

- Discord Thread: @https://discord.com/channels/974306760171073556/1346950985121009776
