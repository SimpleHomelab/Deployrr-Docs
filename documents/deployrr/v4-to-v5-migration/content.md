# v4 to v5 Migration

## Overview

Deployrr v5.0 introduces significant improvements and new features that required a substantial codebase rewrite. Due to the extensive changes, backward compatibility could not be maintained. This guide will help you migrate from v4.6.1 (or older versions) to v5.0.

## New Features in v5.0

- **Brand New Identity**: Fresh Deployrr logo and icon design
- **Local Mode Support**: 
  - Install apps for local access without requiring a reverse proxy
  - Enables multi-server setups by removing the Traefik dependency
- **Enhanced Traefik Integration**:
  - New exposure modes for better access control
  - Simple Mode: All apps behind Traefik are accessible both internally and externally
  - Advanced Mode: Granular control over app exposure (internal, external, or both)
- **Improved Reverse Proxy Configuration**:
  - Traefik now uses file providers by default for app exposure
  - Legacy Docker label method retained for specific apps (Traefik, OAuth, Authelia)

## Migration Steps

> ⚠️ **Important**: Follow these steps carefully to ensure a smooth migration.

1. **Prepare for Migration**
   - Stop all running containers or bring down the entire stack
   - Keep the stack down throughout the migration process
   
2. **Backup Your Data**
   - Execute a cold backup using the Deployrr Tools menu
   - If available, perform additional backups (e.g., Proxmox VM/LXC backup)
   
3. **Clean Up Old Configuration**
   - Exit Deployrr v4.6.1
   - Remove all Deployrr Status files in `/opt/deployarr/status` that start with:
     - `03_`
     - `04_`
     - `05_`
     - `06_`
   - Delete the following from your Docker folder:
     - The compose folder
     - The master Docker compose file

4. **Reinstall and Configure**
   - Restart Deployrr v4.6.1
   - Follow all setup steps in sequence
   - Your existing appdata should be automatically detected
   - When prompted to reinstall Traefik, choose to overwrite

## Additional Resources

For a detailed walkthrough of this migration process, watch our [video guide](https://youtu.be/_9C7bDPveMg).

## Need Help?

If you encounter any issues during migration, please refer to our [support resources](https://docs.deployrr.app/deployarr/get-support) or [Discord community](https://www.simplehomelab.com/discord/) for assistance.