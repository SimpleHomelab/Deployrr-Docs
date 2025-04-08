# Jellyfin Client on LG WebOS

When running Jellyfin behind a Traefik reverse proxy with SSL certificates, some clients like LG WebOS TVs and Xbox consoles may experience connectivity issues. This guide will help you resolve these problems.

## Common Issues

### Connection Error -27

Some users may encounter a "-27" error code when trying to connect to their Jellyfin server from the LG WebOS TV app or Xbox app, even though:
- Web browser access works fine on various devices (phones, computers, etc.)
- The server is accessible both locally and remotely
- Other Jellyfin clients work without issues

This error typically occurs before reaching the login page and is often related to [Traefik's secure headers](https://www.simplehomelab.com/traefik-v3-docker-compose-guide-2024/#Security_Headers) middlware.

## Solution: Modify Traefik Secure Headers

The main solution involves modifying Traefik's secure headers middleware configuration to remove the `customFrameOptionsValue: SAMEORIGIN` header, which can interfere with the Jellyfin client on LG WebOS TVs.

### Steps to Fix

1. Locate your Traefik middleware configuration file (typically `middlewares-secure-headers.yml` inside Traefik rules folder)
2. Comment out or remove the `SAMEORIGIN` line from the `customFrameOptionsValue` configuration
3. Restart Traefik to apply the changes

Here's an example of the modified secure headers middleware created by Deployrr (`customFrameOptionsValue` line must be commented out as shown below):

```yaml
http:
  middlewares:
    middlewares-secure-headers:
      headers:
        accessControlAllowMethods:
          - GET
          - OPTIONS
          - PUT
        accessControlMaxAge: 100
        hostsProxyHeaders:
          - "X-Forwarded-Host"
        stsSeconds: 63072000
        stsIncludeSubdomains: true
        stsPreload: true
        forceSTSHeader: true # This is a good thing but it can be tricky. Enable after everything works.
        # customFrameOptionsValue: SAMEORIGIN # https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
        contentTypeNosniff: true
        browserXssFilter: true
        referrerPolicy: "same-origin"
        permissionsPolicy: "camera=(), microphone=(), geolocation=(), payment=(), usb=()"
        customResponseHeaders:
          X-Robots-Tag: "none,noindex,nofollow,noarchive,nosnippet,notranslate,noimageindex" # disable search engines from indexing home server
          server: "" # hide server info from visitors
        customRequestHeaders:
          X-Forwarded-Proto: https
```

## Additional Tips

- When entering the server address in the LG TV app, try both with and without `https://` and port `:443`

## Related Information

- This configuration change is safe to apply and won't negatively impact other clients
- The modified secure headers will still maintain a strong security posture for your Jellyfin instance
- This solution has been confirmed to work for both LG WebOS TVs and Xbox consoles experiencing similar issues

## Sources

- Discord Discussion: [LG WebOS TV connection issues](https://discord.com/channels/974306760171073556/1159825615147511901/1352742863594586112)
- Discord Discussion: [Traefik secure headers modification](https://discord.com/channels/974306760171073556/1347615487932235817)
