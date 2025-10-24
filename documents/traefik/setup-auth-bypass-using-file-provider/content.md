# Setting Up Traefik Auth Bypass Using File Provider

This guide explains how to set up Traefik authentication bypass for applications when using the file provider instead of Docker labels. This is particularly useful for applications running on remote systems or when you need to bypass Authelia authentication for specific applications like mobile apps.

## Configuration Steps

### 1. Set Auth Bypass Key

1. Open Deployrr
2. Navigate to Reverse Proxy > Auth Bypass
3. Set your desired bypass key

### 2. Configure Traefik Environment

Edit your Traefik compose file:

```yaml
# ~/docker/compose/<servername>/traefik.yml
services:
  traefik:
    environment:
      - TRAEFIK_AUTH_BYPASS_KEY=${TRAEFIK_AUTH_BYPASS_KEY}
```

### 3. Create Router Rules

Create or edit your application's router file:

```yaml
# ~/docker/appdata/traefik3/rules/<servername>/app-<appname>.yml
http:
  routers:
    <appname>-rtr-bypass:
      rule: "Host(`<appname>.{{env "DOMAINNAME_1"}}`) && Header(`traefik-auth-bypass-key`, `{{env "TRAEFIK_AUTH_BYPASS_KEY" }}`)"
      entryPoints:
        - websecure-external
        - websecure-internal
      middlewares:
        - chain-no-auth
      service: <appname>-svc
      priority: 100

    <appname>-rtr:
      rule: "Host(`<appname>.{{env "DOMAINNAME_1"}}`)"
      entryPoints:
        - websecure-external
        - websecure-internal
      middlewares:
        - chain-authelia
      service: <appname>-svc
      tls:
        certResolver: dns-cloudflare
        options: tls-opts@file
      priority: 99

  services:
    <appname>-svc:
      loadBalancer:
        servers:
          - url: "http://<your-app-ip>:<port>/"
```

Replace the following placeholders:
- `<appname>`: Your application name
- `<servername>`: Your server name
- `<your-app-ip>`: Your application's IP address
- `<port>`: Your application's port

### 4. Apply Changes

1. Recreate the Traefik container to apply the environment variable changes:
   ```bash
   sudo docker compose -f /home/USER/docker/docker-compose-HOSTNAME.yml up -d traefik
   ```
   
   Or using the bash alias:
   ```bash
   dcrec traefik
   ```

### 5. Troubleshooting

If you encounter issues:

1. Check Traefik logs:
   ```bash
   tail -f ~/docker/logs/<servername>/traefik/traefik.log
   ```

2. Verify the configuration:
   - Ensure the bypass key is correctly set in Deployrr
   - Confirm the environment variable is properly added to Traefik
   - Check that the router rules are correctly formatted
   - Verify the application's IP and port are correct

## Example Configuration

Here's a complete example for an Immich application:

```yaml
http:
  routers:
    immich-rtr-bypass:
      rule: "Host(`immich.{{env "DOMAINNAME_1"}}`) && Header(`traefik-auth-bypass-key`, `{{env "TRAEFIK_AUTH_BYPASS_KEY" }}`)"
      entryPoints:
        - websecure-external
        - websecure-internal
      middlewares:
        - chain-no-auth
      service: immich-svc
      priority: 100

    immich-rtr:
      rule: "Host(`immich.{{env "DOMAINNAME_1"}}`)"
      entryPoints:
        - websecure-external
        - websecure-internal
      middlewares:
        - chain-authelia
      service: immich-svc
      tls:
        certResolver: dns-cloudflare
        options: tls-opts@file
      priority: 99

  services:
    immich-svc:
      loadBalancer:
        servers:
          - url: "http://192.168.30.23:2283/"
```

## Sources

- [Discord Discussion](https://discord.com/channels/974306760171073556/1380058913663357069)
