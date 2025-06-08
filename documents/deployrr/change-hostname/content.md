# Change Hostname

When you change your server's hostname after Deployrr installation, you may encounter prerequisite errors when trying to manage containers. This guide explains how to properly update Deployrr to reflect the new hostname.

## Symptoms

After changing your server hostname and updating the `.env` file, you may see **Prerequisites Not Met** error.  

## Automated Solution (v5.9+)

For Deployrr v5.9 and later, use the automated **Change Hostname** tool:

1. Navigate to the **Tools** menu in Deployrr
2. Select **Change Hostname**
3. Follow the prompts to update all necessary files and folders

## Manual Solution

If using an older version or prefer manual steps, follow these procedures:

> **Warning:** Replace `USER` and `HOSTNAME` with your actual username and hostname throughout these steps.

### 1. Stop All Running Containers
Stop all running Docker containers:

```bash
sudo docker stop $(docker ps -q)
```

### 2. Update Environment File

Edit the `.env` file and update the value of `HOSTNAME` variable to the new hostname ("Let's call this `NEWHOSTNAME`"):

```bash
nano /home/USER/docker/.env
```

### 3. Rename Docker Compose File

Rename the main Docker Compose file to match the new hostname:

```bash
mv /home/USER/docker/docker-compose-OLDHOSTNAME.yml /home/USER/docker/docker-compose-NEWHOSTNAME.yml
```

### 4. Rename Directory Structures

Update the following directories to reflect the new hostname:

**Logs directory:**
```bash
mv /home/USER/docker/logs/OLDHOSTNAME /home/USER/docker/logs/NEWHOSTNAME
```

**Compose directory:**
```bash
mv /home/USER/docker/compose/OLDHOSTNAME /home/USER/docker/compose/NEWHOSTNAME
```

**Traefik rules directory:**
```bash
mv /home/USER/docker/appdata/traefik3/rules/OLDHOSTNAME /home/USER/docker/appdata/traefik3/rules/NEWHOSTNAME
```

### 5. Restart Services

After making these changes, restart your Docker services:

```bash
sudo docker compose -f /home/USER/docker/docker-compose-NEWHOSTNAME.yml up -d --force-recreate
```

## Additional Support

If you continue experiencing issues, visit our [support page](https://docs.deployrr.app/deployrr/get-support) or join our [community Discord](https://www.simplehomelab.com/discord/).

---

## Sources

- [Discord Discussion](https://discord.com/channels/974306760171073556/1380986201879609515)
