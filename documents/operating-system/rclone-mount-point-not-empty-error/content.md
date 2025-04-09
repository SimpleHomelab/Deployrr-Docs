# Rclone "Mount Point Not Empty" Error

When using rclone mounts in Deployrr, you might encounter an error stating that the mount point is not empty. This is a safety feature implemented to prevent accidental data loss. This guide will help you understand and resolve this issue.

## Understanding the Error

When you see the following error in your rclone logs (/media/rclone/NAS is just an example):

```
Fatal error: failed to mount FUSE fs: "/media/rclone/NAS" is not empty, use --allow-non-empty to mount anyway
```

This means that the directory where rclone is trying to mount your remote storage already contains files or folders. This is a safety measure to prevent accidentally mounting over existing content.

## Common Causes

1. Previous service data or metadata
2. Incomplete unmounting of previous mounts
3. Running services creating files in the mount directory (e.g. Jellyfin, Radarr, etc. working while the remote folder is not mounted) 

## How to Fix

1. First, ensure your stack is down to prevent any services from accessing the mount point:
   ```bash
   sudo docker compose -f /home/USER/docker/docker-compose-HOSTNAME.yml down
   ```
   Using Anand's Bash Alias:
   ```bash
   dcdown
   ```

2. Check the contents of your mount point directory:
   ```bash
   ls /media/rclone/NAS
   ```

3. Verify that the files in the mount point are not important. The actual NAS contents should not be visible yet since the mount hasn't succeeded.

4. Remove the contents of the mount point:
   ```bash
   sudo rm -rf /media/rclone/NAS/*
   ```

5. Try the automount again using Deployrr.

## Important Warning

⚠️ **Before deleting any files:**
- Always verify you're working in the correct directory
- Ensure the mount point isn't currently mounted
- Confirm the files you're deleting aren't your actual NAS contents
- Make sure your stack is down to prevent services from accessing the mount point

## Troubleshooting

If you encounter permission errors while trying to delete files:

1. Ensure your stack is completely down
2. Use sudo for the removal command
3. Check if any processes are still accessing the directory

For additional support, visit our [support page](https://docs.deployrr.app/deployrr/get-support) or join our [community](https://www.simplehomelab.com/discord/).

## Sources

- Discord Thread: https://discord.com/channels/974306760171073556/1350240462593589278
