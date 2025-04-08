# Additional Instances of *arr Applications

This guide explains how to set up additional instances of *arr applications (like Radarr, Sonarr, or Readarr) in your Docker environment. This is useful when you want to have separate instances for different purposes, such as having Radarr-4K alongside your standard Radarr instance.

> **Important**: Throughout this guide, replace `USER` and `HOSTNAME` with your specific values. The examples assume your Docker root folder is `/home/USER/docker` - adjust this path according to your setup.

## Prerequisites

- Docker and Docker Compose installed
- Existing *arr application setup in your Docker environment
- Basic understanding of Docker Compose YAML files

## Steps to Add Another Instance

1. **Copy the Existing Service Configuration**
   - Locate the compose file for first instance (e.g. `/home/USER/docker/compose/HOSTNAME/radarr.yml`)
   - Make a copy of the file and rename it (e.g. `/home/USER/docker/compose/HOSTNAME/radarr-4k.yml`)

2. **Modify the New Service**
   - Change the service name (e.g., from `radarr` to `radarr-4k`)
   - Update the container name (e.g., from `radarr` to `radarr-4k`)
   - Customize the volumes to use different paths
   - Ensure ports don't conflict with existing services

### Example Configuration

Here's an example of adding a Radarr-4K instance alongside the standard Radarr:

```yaml
# Original Radarr Service
radarr:
  container_name: radarr
  ...
  ports:
    - "7878:7878"
  ...
  volumes:
    - /home/USER/docker/appdata/radarr:/config
...
```

```yaml
# Additional Radarr-4K Service
radarr-4k:
  container_name: radarr-4k
  ...
  ports:
    - "7879:7878"  # Note the different external port
  ...
  volumes:
    - /home/USER/docker/appdata/radarr-4k:/config
  ...
```

## Add the New Service to Master Docker Compose

- Open your master docker compose file (e.g. `/home/USER/docker/docker-compose-HOSTNAME.yml`)
- Add the new service (radarr-4k.yml) to the list of includes

## Create the New Service

```bash
# Create or Recreate a service
sudo docker compose -f /home/USER/docker/docker-compose-HOSTNAME.yml up -d --force-recreate radarr-4k
```

### Using Anand's Bash Aliases

For convenience, you can use these simplified commands:

```bash
# Create or Recreate a specific service
dcrec radarr-4k
```

Reference: [Simplify Docker, Docker Compose, and Linux Commands with Bash Aliases](https://www.simplehomelab.com/bash-aliases-for-docker-and-linux/)

## Add Reverse Proxy Configuration

- If you use Traefik file providers to create reverse proxy routes (default behavior in Deployrr), then copy paste the existing file provider (e.g. `app-radarr.yml`) in Traefik rules folder
- Edit references to original service name (e.g. `radarr`)
- Edit address to update the port number for the new service

## Important Notes

- Ensure different external ports for each instance
- Use distinct configuration directories for each instance
- Update any reverse proxy configurations if needed
- Make sure to create the new config directories before starting the services

## Sources

- Discord Thread: @https://discord.com/channels/974306760171073556/1349207158113173535
