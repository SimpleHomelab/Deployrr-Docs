# Multi-Server Setup with Same Domain

This guide explains how to use Deployrr across multiple servers while maintaining services under the same domain (e.g., `*.yourdomain.com`).

## Overview

You can run Deployrr across multiple servers while keeping all services under the same domain. This setup is useful when you have:
- Different servers optimized for specific tasks (e.g., storage, computation)
- Need to distribute workloads across multiple machines
- Want to maintain a unified domain structure

## Architecture

In this setup:
1. Primary Server (Machine 1):
   - Runs Deployrr in hybrid mode
   - Hosts the main Traefik instance
   - Manages authentication
   - Acts as the central reverse proxy

2. Secondary Server(s) (Machine 2+):
   - Runs Deployrr in local mode
   - Hosts applications
   - Connects back to the primary server's Traefik

## Setup Instructions

### Primary Server Setup

1. Install Deployrr in hybrid mode on your first machine
2. Configure Traefik as your reverse proxy
3. Set up authentication

### Secondary Server Setup

1. Install Deployrr in local mode
2. Deploy your applications normally
3. Use the `traefikify` option to create Traefik file providers for each app you want to expose

### Connecting Services

For each application on secondary servers that you want to expose:

1. On the primary server, create Traefik file providers for the secondary server's applications
2. Use the `traefikify` option to generate the correct configuration
3. Ensure network connectivity between servers

## Example Use Case

Consider this scenario:
- `storage.domain.com` → Ubuntu server with 2TB disk for storage-heavy apps
- `compute.domain.com` → Ubuntu server for other self-hosted applications

## Best Practices

- Keep the primary Traefik instance on the most reliable/stable server

## Sources

- Discord Discussion: [Multi-Server Setup Discussion](https://discord.com/channels/974306760171073556/974314326494171226/1344756299841146984)

