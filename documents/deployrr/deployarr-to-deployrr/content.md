# Migrating from Deployarr to Deployrr

Deployarr was renamed to **Deployrr** starting with version 5.7.1. This guide walks you through the migration process to ensure your settings and configurations are properly transferred to the new naming convention.

## Important Notes

> **Warning**: Customize `USER` and `HOSTNAME` in all commands below to match your specific environment. The examples assume your docker root folder is `/home/USER/docker` - adjust if your setup differs.

## Migration Requirements

- Deployarr v5.7 (final version under old name)
- Must upgrade through v5.7.1 before updating to latest/future versions

## Migration Steps

### Step 1: Install Deployarr v5.7

First, obtain and run the final Deployarr version to ensure compatibility:

```bash
# Navigate to your home directory
cd /home/USER

# Download Deployarr v5.7
wget http://www.deployrr.app/archives/deployarr_v5.7.app

# Make executable
sudo chmod +x deployarr_v5.7.app

# Run the application
./deployarr_v5.7.app
```

**Important**: Once the application starts, navigate through some menus to ensure proper initialization. You don't need to make any configuration changes - just access a few menu options then exit.

### Step 2: Install Deployrr v5.7.1 (Migration Version)

Now install the renamed version which will automatically migrate your settings:

```bash
# Download Deployrr v5.7.1 (new naming)
wget http://www.deployrr.app/archives/deployrr_v5.7.1.app

# Make executable
sudo chmod +x deployrr_v5.7.1.app

# Run the migration version
./deployrr_v5.7.1.app
```

This version will detect your existing Deployarr settings and migrate them to the new Deployrr naming convention automatically.

### Step 3: Update to Latest Version

After performing the above steps, you may upgrade to any new version any time. 

## Cleanup (Optional)

After successful migration, you can remove the old installation files:

```bash
sudo rm deployarr_v5.7.app
sudo rm deployrr_v5.7.1.app
```