# Docker Compose Malformed

When setting up Deployrr on Debian 12, you might encounter a malformed Docker Compose file error. This is a known issue specific to Debian 12 installations.

## Error Message

You may encounter this error during the Docker environment setup:

```plaintext
Creating starter docker-compose-om-deb.yml file...
[ERROR] /home/USERNAME/docker/docker-compose-om-deb.yml is malformed.
If needed, you may generate a troubleshooting log from the Settings menu.
```

## Solution

If you encounter the malformed Docker Compose error:
1. Delete the malformed Docker Compose file:
   ```bash
   rm ~/docker/docker-compose-om-deb.yml
   ```
2. Return to the main menu
3. Re-run the Docker environment setup

## Important Notes

- This issue is specific to Debian 12 installations
- Do not perform a fresh installation if you encounter this error
- Simply delete the malformed file and retry the environment setup
- Pay attention to on-screen messages during installation for troubleshooting steps

## Sources

- Discord Thread: https://discord.com/channels/974306760171073556/1343085128913518726/1343085128913518726
