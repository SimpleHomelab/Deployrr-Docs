# Restrict Access Using IP Allowlist

When running services behind Traefik with SSL enabled, you might want to restrict access to certain IP ranges (e.g., internal networks) while blocking external access. This guide demonstrates how to implement IP-based access control using Traefik's middleware functionality.

> **Tip**: IP allowlisting is just one approach to restricting access. Another effective method is using separate internal and external endpoints (websecure-internal and websecure-external). This is the approach that Deployrr uses - internal services are only exposed on the internal endpoint, which is only accessible from your local network. However, for this to work, DNS hairpinning or local split DNS is required. 

## Prerequisites

- Traefik v3.x configured and running
- Basic understanding of Traefik middlewares and configurations
- Services already set up with SSL certificates through Traefik

## Implementation

There are two methods to implement IP restrictions: using Docker labels or using Traefik's file provider. Choose the method that best suits your setup.

### 1. Create IP Allowlist Middleware

Create a new file called `middlewares-ipallowlist.yml` in your Traefik rules directory. This is typically `/home/user/docker/appdata/traefik3/rules/HOSTNAME/` if you have followed Anand's guides, videos, or used Deployrr:

```yaml
http:
  middlewares:
    middlewares-ipallowlist:
      ipAllowList:
        sourceRange:
          - "127.0.0.1/32"      # Localhost
          - "192.168.100.0/22"  # Internal network range
          - "10.40.10.0/24"     # Additional internal network
          - "10.40.100.0/24"    # Additional internal network
```

This configuration creates a middleware that only allows access from the specified IP ranges. Adjust the `sourceRange` values according to your network configuration.

### 2. Create a Middleware Chain

Create a file called `chain-no-auth-internal-only.yml` in the Traefik rules folder. You may also add the above middleware to existing chains if you prefer:

```yaml
http:
  middlewares:
    chain-no-auth-internal-only:
      chain:
        middlewares:
          - middlewares-ipallowlist
```

This chain allows you to combine multiple middlewares if needed in the future.

### 3. Apply the Middleware

#### Method 1: Using Docker Labels

Add the middleware chain to your service's labels in the docker-compose file:

```yaml
services:
  your-service:
    labels:
      - "traefik.http.routers.your-service.middlewares=chain-no-auth-internal-only@file"
```

#### Method 2: Using File Provider

Create a Traefil file provider for a service (e.g. Proxmox Web Interface; `app-proxmox-webui.yml`) in Traefik rules folder:

```yaml
http:
  routers:
    proxmox-webui-rtr:
      rule: "Host(`proxmox.{{env "DOMAINNAME_1"}}`)" 
      entryPoints:
        - websecure-internal # Only use internal endpoint for restricted access
        - websecure-external # For apps exposed externally
      middlewares:
        - chain-no-auth-internal-only
      service: proxmox-webui-svc
      tls:
        certResolver: dns-cloudflare
        options: tls-opts@file
  services:
    proxmox-webui-svc:
      loadBalancer:
        passHostHeader: true
        serversTransport: "proxmox-webui-st"
        servers:
          - url: "https://192.168.5.110:8006"  # https://IP-ADDRESS:PORT
  serversTransports:
    proxmox-webui-st:
      insecureSkipVerify: true
```

Key points for the file provider method:
- Replace `proxmox-webui` with your application name
- Adjust the hostname `proxmox` in the `rule` field
- Update the service URL to match your application's internal address

## Behavior

- **Internal Access**: Users accessing the service from the allowed IP ranges will be able to connect normally
- **External Access**: Users attempting to access from non-allowed IP addresses will receive a "403 Forbidden" response

## Common IP Ranges

Here are some common internal IP ranges you might want to allow:

- `127.0.0.1/32`: Localhost
- `192.168.0.0/16`: Common home/office network range
- `10.0.0.0/8`: Private network range
- `172.16.0.0/12`: Private network range

## Troubleshooting

If you encounter issues, [check logs](https://www.simplehomelab.com/traefik-v3-docker-compose-guide-2024/#Learn_to_Read_Logs):

1. Check your Traefik logs for any middleware-related errors
2. Verify that your IP ranges are correctly formatted
3. Ensure the middleware files are properly loaded by Traefik
4. Confirm that your current IP falls within the allowed ranges
5. For file provider method, verify that Traefik is watching the correct directory
6. Check that the entry points specified in your configuration exist in your Traefik setup

## Credit

- Discord User: supersurf1000 (YMK)
