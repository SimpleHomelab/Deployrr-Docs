# Getting Support via SSH

When you need direct assistance with your Deployrr setup, Anand (depending on availability) can provide remote support via SSH. This method allows for quick troubleshooting and problem resolution.

> ⚠️ **Security Warning**: Providing SSH access gives visibility to your system contents. Only grant access when necessary and follow the cleanup steps after support is complete.

## Prerequisites

Before proceeding, ensure you have:
- Access to your router's port forwarding settings
- Administrative access to your system
- Discord account to message Anand

## Setup Steps

### 1. Check SSH Port

First, verify your SSH port configuration:

```bash
sudo cat /etc/ssh/sshd_config | grep "Port "
```

The default SSH port is 22. Note down your configured port number.

### 2. Configure Port Forwarding

Forward the SSH port from your router to your Docker host:
1. Log into your router's admin interface
2. Navigate to port forwarding settings
3. Create a new port forwarding rule:
   - External Port: Your SSH port (e.g., 22)
   - Internal IP: Your Docker host's IP address
   - Internal Port: Your SSH port (e.g., 22)
   - Protocol: TCP

### 3. Configure Firewall (if applicable)

If you're using UFW (Uncomplicated Firewall), allow the SSH port:

```bash
sudo ufw allow <ssh-port>/tcp
```

Replace `<ssh-port>` with your actual SSH port number.

### 4. Set Temporary Password

Set a temporary password for the non-root user that Anand will use:

```bash
sudo passwd <user>
```

Replace `<user>` with your non-root username. 

> 🔒 **Security Note**: For simplicity, this guide uses password authentication. Key-based authentication is more secure but requires additional setup.

### 5. Share Access Details

Send the following information to Anand via **Direct Message** on [Discord](https://www.simplehomelab.com/discord/):
- Public/WAN IP address
- SSH port number
- Username
- Temporary password

> ⚠️ **Important**: Never share these credentials publicly in Discord channels or forums. Only send them via Direct Message to Anand (that is if you trust him).

## After Support

Once the troubleshooting is complete:

1. Remove the port forwarding rule from your router
2. Change the user password:
   ```bash
   sudo passwd <user>
   ```

## Future Improvements

🔔 **Coming Soon**: Future versions of Deployrr will include a more secure remote support option using Tailscale, providing enhanced security and easier setup.

## Need Help?

If you need additional assistance, check our [support documentation](https://docs.deployrr.app/deployrr/get-support) or join our [community Discord](https://www.simplehomelab.com/discord/).
