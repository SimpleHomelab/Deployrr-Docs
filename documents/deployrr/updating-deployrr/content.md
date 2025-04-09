# Updating Deployrr

When you start Deployrr, the system automatically checks for new versions. 

> ⚠️ **Warning**: New versions of Deployrr may include breaking changes that could require:
> - Reinstallation of certain applications
> - Minor manual edits to your configuration
> - Updates to your existing setup
>
> Always check the [CHANGELOG](https://github.com/SimpleHomelab/deployrr/blob/main/CHANGELOG.md) before upgrading and make sure to backup your configuration.

![]({{DOC_PATH}}update_deployrr1.png)

If a newer version is available, you'll receive a notification like this:

```bash
[WARNING] You are running version 5.6. There is a new version: 5.7.
```

## Upgrade Process

1. After the notification, you'll be prompted to download the new version:

```bash
Download 5.7 (will be saved alongside this version)? Press Y/y to download v5.7 or any other key to continue with v5.6:
```

2. If you press `Y` or `y`:
   - The new version (v5.7) will be downloaded
   - It will be saved in the same location as your current version

![]({{DOC_PATH}}update_deployrr2.png)

3. After the download completes, you'll be asked if you want to switch to the new version:

```bash
Do you want to exit v5.6 and start the newer v5.7 (Y/N)?
```

4. If you press `Y` or `y`:
   - The current version (v5.6) will be stopped
   - The new version (v5.7) will start automatically
   - Your configuration will be preserved

![]({{DOC_PATH}}update_deployrr3.png)

If you choose not to upgrade or switch versions at any point, Deployrr will continue running with your current version.

## Notes

- The new version is downloaded alongside your existing version, ensuring a safe upgrade process
- Your configuration and settings are preserved when upgrading
- You can always choose to continue using your current version if needed
- It's recommended to regularly update Deployrr to get the latest features and security improvements
