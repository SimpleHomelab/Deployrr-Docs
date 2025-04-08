# Fixing Internet Connectivity Issues on Debian 12

When running Deployrr on Debian 12, you might experience a loss of internet connectivity after running the setup script. This is a known DNS configuration issue specific to Debian 12 installations.

## Error Message

You may encounter this error during the setup process:

```plaintext
[ERROR] Problem with some dependencies. This can be due to two reasons:
---> 1) No internet connection - ensure there is internet connection on your server.
---> 2) Deployrr server is down for maintenance
```

Running `sudo apt update` will fail, confirming the loss of internet connectivity.

## Solution

To resolve the internet connectivity issue:

1. Edit the systemd resolved configuration:
   ```bash
   sudo nano /etc/systemd/resolved.conf
   ```

2. Add or modify these DNS settings:
   ```ini
   DNS=1.1.1.1
   FallbackDNS=8.8.8.8
   ```

3. Reboot your system:
   ```bash
   sudo reboot
   ```

4. After the system comes back online, retry the Deployrr setup

## Important Notes

- This issue is specific to Debian 12 installations
- The problem occurs due to DNS configuration changes during setup
- Using Cloudflare DNS (1.1.1.1) and Google DNS (8.8.8.8) provides reliable connectivity
- Watch for setup messages that may provide additional troubleshooting steps

## Sources

- Discord Thread: https://discord.com/channels/974306760171073556/1343085128913518726/1343085128913518726
