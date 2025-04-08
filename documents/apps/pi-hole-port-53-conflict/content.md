# Pi-hole Port 53 Conflict

When deploying Pi-hole on Docker using Deployrr, you might encounter an error indicating that port 53 is already in use. This typically occurs because `systemd-resolved` is listening on port 53, which Pi-hole needs for DNS services.

## Error Message

You might see an error similar to this when trying to deploy Pi-hole:

```bash
Error response from daemon: driver failed programming external connectivity on endpoint pihole: failed to bind host port for 0.0.0.0:53:172.18.0.18:53/tcp: address already in use
```

## Solution

To resolve this issue, follow these steps:

1. Edit the systemd-resolved configuration file:
   ```bash
   sudo nano /etc/systemd/resolved.conf
   ```

2. Add or modify the following line in the configuration:
   ```ini
   DNSStubListener=no
   ```

3. Restart the systemd-resolved service:
   ```bash
   sudo systemctl restart systemd-resolved
   ```

4. "Full" delete PiHole using Deployrr Stack Manager

5. Redeploy Pi-hole through Deployrr

After following these steps, Pi-hole should deploy successfully with port 53 available for use.

## Sources

- Discord Thread: @https://discord.com/channels/974306760171073556/1346884572138438667/1347161142794911825
