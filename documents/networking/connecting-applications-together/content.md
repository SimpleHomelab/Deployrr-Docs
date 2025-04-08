# Connecting Applications Together

When deploying multiple Arr applications (Radarr, Sonarr, Lidarr, etc.) using Deployrr, you'll need to properly configure them to communicate with each other. This guide explains the recommended approach for connecting these applications.

## Best Practices

### Using Container Names (Recommended)

By default, all Deployrr applications belong to the "default" Docker network. This allows you to use container names (hostnames) and internal ports for communication between services.

Use the following format to connect your applications:
```
hostname:port
```

Here are the default container names and ports for common Arr applications:

| Application | Connection String | Port |
|------------|------------------|------|
| Radarr     | `radarr:7878`    | 7878 |
| Sonarr     | `sonarr:8989`    | 8989 |
| Bazarr     | `bazarr:6767`    | 6767 |
| Prowlarr   | `prowlarr:9696`  | 9696 |
| Lidarr     | `lidarr:8686`    | 8686 |
| SABnzbd    | `sabnzbd:8080`   | 8080 |

### Alternative Method: Using Docker Host IP

If needed, you can alternatively use the Docker host IP address with the configured ports:
```
DOCKER-HOST-IP:PORT
```

The port number should match the one set in your service's environment variables (e.g., `RADARR_PORT` in your `.env` file).

## Important Notes

- **Do not use FQDNs**: When connecting Arr applications together, avoid using Fully Qualified Domain Names (FQDNs). Using FQDNs will route traffic through your authentication middleware (like Authentik or Authelia), which can cause connection issues.
- **Avoid Container IPs**: Do not use Docker container IPs for connections between applications. Container IPs are not static and will change when containers are restarted, making this method unreliable. While you may work around this be manually giving the container a static IP, the above 2 methods are better.
- **Network Configuration**: All applications deployed through Deployrr are automatically added to the "default" network, enabling hostname-based communication.

## Sources

- Discord Discussion: [Connecting Arr Apps](https://discord.com/channels/974306760171073556/1345534515736612985)
