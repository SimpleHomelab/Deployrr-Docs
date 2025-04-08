# Theme Park Configuration

This guide explains how to configure Theme Park with Traefik in Deployrr. While written for Deployrr, these steps can be adapted for other Docker Compose setups.

## Configuration Steps

### 1. Navigate to Traefik Rules Directory

Navigate to your Traefik rules folder:

```bash
cd /home/admin/docker/appdata/traefik3/rules/MediaServer
```

### 2. Create Theme Middleware

Create a new middleware file for each application you want to theme:

```bash
nano theme-qbittorrent.yml
```

Add the following configuration:

```yaml
http:
  middlewares:
    theme-qbittorrent: # the name of your middleware
      plugin:
          traefik-themepark:
              app: qbittorrent # your app
              theme: aquamarine # the theme you chose
```

Repeat this process for each application, adjusting the app name and theme as needed.

### 3. Update Application Rules

Edit your application's configuration file:

```bash
nano app-qbittorrent.yml
```

Add the theme middleware under the middlewares section:

```yaml
- theme-qbittorrent
```

### 4. Configure Traefik

Navigate to your Traefik compose directory:

```bash
cd /home/user/docker/compose/MediaServer/
```

Edit the Traefik configuration:

```bash
nano traefik.yml
```

Add the following under the `command:` section:

```yaml
- --experimental.plugins.traefik-themepark.modulename=github.com/packruler/traefik-themepark
- --experimental.plugins.traefik-themepark.version=v1.4.2
```

Add the middleware label for each themed application:

```yaml
- "traefik.http.routers.gluetun-qbittorrent-rtr.middlewares=theme-qbittorrent@file" # qBittorrent
```

### 5. Application-Specific Configuration

#### qBittorrent

In qBittorrent's WebUI settings (WebUI > Add custom HTTP headers), add:

```
content-security-policy: default-src 'self'; style-src 'self' 'unsafe-inline' theme-park.dev raw.githubusercontent.com use.fontawesome.com; img-src 'self' theme-park.dev raw.githubusercontent.com data:; script-src 'self' 'unsafe-inline'; object-src 'none'; form-action 'self'; frame-ancestors 'self'; font-src use.fontawesome.com;
```

### 6. Apply Changes

Restart Traefik to apply the configuration.

## Sources

- [Discord Thread Discussion](https://discord.com/channels/974306760171073556/1271721488038367272/1271721488038367272)
