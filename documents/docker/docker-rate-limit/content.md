# Docker Pull Rate Limits

When working with Docker containers, you might encounter rate limit errors while pulling images from Docker Hub. This document explains what these errors are, why they occur, and how to resolve them.

## Understanding the Issue

Users may encounter errors like the following when pulling multiple Docker images (e.g. while using `sudo docker compose up` command or `dcpull` bash alias):

```bash
[+] Pulling 16/16
✔ sonarr Pulled
✔ scrutiny Pulled
✔ ddns-updater Pulled
✘ portainer Error  Get "https://registry-1.docker.io/v2/": net/http: TLS handshake timeout
...
✘ influxdb Error  Get "https://registry-1.docker.io/v2/": net/http: TLS handshake timeout
...
✘ traefik Error  Get "https://registry-1.docker.io/v2/": net/http: TLS handshake timeout
...
```

These errors can appear randomly on different images when repeatedly trying to pull them. This is typically due to Docker Hub's rate limiting policy.

## Docker Hub Rate Limits

Docker Hub implements rate limits on image pulls:

- **Anonymous Users**: Limited to 100 container image pulls per 6 hours
- **Authenticated Users**: Limited to 200 container image pulls per 6 hours
- **Docker Pro/Team Subscribers**: Higher limits available based on subscription

## Solution

To resolve rate limit errors, follow these steps:

1. Create a Docker Hub account at [hub.docker.com](https://hub.docker.com)
2. Authenticate your Docker client by running:
   ```bash
   sudo docker login
   ```
3. Follow the prompts to authenticate yourself and the **Login Successful** message. After authenticating, you should be able to pull images without encountering rate limit errors.

## Example Output

```bash
[anand@mds: $~]$ sudo docker login
[sudo] password for anand:

USING WEB-BASED LOGIN

i Info → To sign in with credentials on the command line, use 'docker login -u <username>'

Your one-time device confirmation code is: RGHQ-LQDF
Press ENTER to open your browser or submit your device code here: https://login.docker.com/activate

Waiting for authentication in the browser…

WARNING! Your credentials are stored unencrypted in '/root/.docker/config.json'.
Configure a credential helper to remove this warning. See
https://docs.docker.com/go/credential-store/

Login Succeeded
```

## Sources

- Discord Discussion: [Docker Pull Rate Limit Discussion](https://discord.com/channels/974306760171073556/1365888310500655124)
- [Official Docker Documentation on Usage and Limits](https://docs.docker.com/docker-hub/usage/)
