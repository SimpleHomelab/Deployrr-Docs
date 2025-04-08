# qBittorrent WebUI Troubleshooting

This guide addresses common issues with accessing the qBittorrent WebUI when using Deployrr, particularly when running with a VPN configuration.

## Common Issues

### Unauthorized Access Error

If you're encountering an "Unauthorized" error when trying to access the qBittorrent WebUI, try these solutions:

1. **Check Host Header Validation**
   - Edit your `qbittorrent.conf` file in /home/user/docker/appdata/qbittorrent (or qbittorrent-vpn)
   - Add the following line under the `[Preferences]` section:
   ```ini
   WebUI\HostHeaderValidation=false
   ```

2. **Reset Default Credentials**
   If you can't access the WebUI with the default credentials (username: `admin`, password: `adminadmin`), you can reset them by:
   
   a. Stop the qBittorrent container (sudo docker stop qbittorrent or dcstop qbittorrent if you use Anand's bash aliases)
   b. Add the following line under `[Preferences]` in your config file:
   ```ini
   WebUI\Password_PBKDF2="@ByteArray(ARQ77eY1NUZaQsuDHbIMCA==:0WMRkYTUWVT9wVvdDtHAjU9b3b7uB8NR1Gur2hmQCvCDpm39Q+PsJRJPaCU51dEiz+dTzh8qbPsL8WkFljQYFQ==)"
   ```
   c. Start the container (sudo docker start qbittorrent or dcstart qbittorrent if you use Anand's bash aliases)
   d. Log in with default credentials (username: `admin`, password: `adminadmin`)
   e. Set your new login details

3. **Check Access URL**
   - Use your host IP address instead of `localhost`
   - Verify the correct port in your `.env` file
   - Access format: `http://your.host.ip:port`

## Essential Configuration

Ensure your `qbittorrent.conf` has these basic settings:

```ini
[Preferences]
WebUI\Address=*
WebUI\ServerDomains=*
WebUI\HostHeaderValidation=false

[BitTorrent]
Session\DefaultSavePath=/downloads/
Session\Port=6881

[Network]
PortForwardingEnabled=false
```

## Additional Resources

- Watch Anand's comprehensive qBittorrent setup playlist: [qBittorrent, Gluetun VPN, and DeUnhealth - The Perfect Trio](https://youtube.com/playlist?list=PL1Hno7tIbSWWZXw1jEsYWPRK-gHqITZds)

## Sources

- Discord Discussion: https://discord.com/channels/974306760171073556/1334338303914147943
