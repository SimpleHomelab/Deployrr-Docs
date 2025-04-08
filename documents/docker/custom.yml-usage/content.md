# custom.yml Usage

The `custom.yml` file provides a way to define and manage Docker Compose configurations that are not part of Deployrr's standard Apps library or when you need custom configurations for existing apps.

## Overview

`custom.yml` serves as a centralized location for your custom Docker Compose configurations. While using this file is optional, it offers a convenient way to manage your custom containers alongside Deployrr's managed applications.

## Usage

1. Locate the `custom.yml` file in your Deployrr configuration directory.

2. Enable the file by uncommenting `custom.yml` in the `include:` section of your main compose file:
   ```yaml
   # In docker-compose-<hostname>.yml
   include:
     - custom.yml
   ```

3. Add your custom Docker Compose configurations to `custom.yml`, following standard Docker Compose syntax.

4. After making changes, apply them using one of these methods:
   - Run `docker compose -f /home/user/docker/docker-compose-<hostname>.yml up <service-name>` to update a specific service
   - Use Anand's aliases: 
     - `dcup <service-name>` to update a service
     - `dcrec <service-name>` to recreate a service

## Alternative Approach

While `custom.yml` is provided for convenience, you're not limited to using it exclusively. As noted by Anand, you can:

1. Create separate YAML files with custom names (e.g., `mycustomapp.yml`)
2. Include these files in your main Docker Compose configuration
3. Manage them independently of Deployrr

## Example Structure

```yaml
# custom.yml
services:
  mycustomapp1:
    image: customimage:latest
    container_name: mycustomapp1
    # ... other configuration options

  mycustomapp2:
    image: anotherimage:latest
    container_name: mycustomapp2
    # ... other configuration options
```

## Sources

- [Discord Discussion](https://discord.com/channels/974306760171073556/1345522868427034685/1345522868427034685)
