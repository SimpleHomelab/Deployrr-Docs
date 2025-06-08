# Home Assistant Behind Traefik

Learn how to expose your Home Assistant instance through Traefik reverse proxy using Deployrr's Traefikify feature (Traefik File Provider).

## Overview

If you're running Home Assistant on a separate VM or container and want to access it remotely through your existing Traefik setup, you can use Deployrr's Traefikify feature to add it behind your reverse proxy.

## Prerequisites

- Deployrr installed and running with Traefik
- Home Assistant instance running (VM, container, or Home Assistant OS)
- Network access between Traefik and Home Assistant
- Admin access to Home Assistant configuration

## Step 1: Use Traefikify in Deployrr

Use the Traefikify feature to add your existing Home Assistant instance behind Traefik:

1. Navigate to the Traefikify section under Reverse Proxy Menu of Deployrr
2. Add your Home Assistant instance with the appropriate configuration
3. Configure the target IP address and port (typically 8123 for Home Assistant)
4. Use http (or https) for protocol

## Step 2: Configure Trusted Proxies in Home Assistant

**Critical:** You must configure trusted proxies in Home Assistant's `configuration.yaml` file for the reverse proxy to work properly.

Add the following configuration to your Home Assistant `configuration.yaml`:

```yaml
http:
...
  use_x_forwarded_for: true
  trusted_proxies:
    - 192.168.1.XXX  # Replace with your Docker/Deployrr host IP
...
```

## Step 3: Identify Your Traefik Host IP

Replace `192.168.1.XXX` in the trusted_proxies section with your actual Traefik host IP address. This should be:

- The IP address of your Ubuntu container running Deployrr/Traefik
- The IP address that Home Assistant will see requests coming from

## SSL Configuration

**Do not enable SSL in Home Assistant** when using it behind Traefik. Traefik will handle SSL termination and certificate management automatically.

## Sources

- [Discord Discussion](https://discord.com/channels/974306760171073556/1380094049729380462)
- [Home Assistant HTTP Integration Documentation](https://www.home-assistant.io/integrations/http/#trusted_proxies)
