# Bash Aliases in Deployrr

Deployrr can install a comprehensive set of bash aliases to simplify system and Docker administration tasks. These aliases provide convenient shortcuts for common operations.

## Installation Process

The bash aliases are installed in two steps:

1. The main aliases file is saved to the `shared/config` folder within your Docker directory
2. A reference to this file is added to your `.bashrc` file to load the aliases on shell startup

> **Note**: The actual path to the Docker folder may vary depending on your setup. By default, it uses `/home/USER/docker` but can be customized during installation.

## Command Categories

### Shell Prompt Configuration

#### PS1 Configuration
```bash
export PS1="[\[\e[32m\]\u\[\e[m\]@\[\e[33m\]\h\[\e[m\]: \\$\[\e[36m\]\w\[\e[m\]]\\$ "
```
Configures a colorized shell prompt showing username (green), hostname (yellow), and current path (cyan) in an easy-to-read format.

### Docker Management

#### Basic Container Operations
- `dps`: Lists all Docker containers (running and stopped) with detailed information.
- `dpss`: Displays containers in a nicely formatted table showing names, state, status, and image.
- `ddf`: Shows Docker disk usage statistics for containers, images, and volumes.
- `dexec`: Executes an interactive shell session in a running container.

#### Container Logs and Monitoring
- `dlogs`: Shows and follows the last 50 lines of container logs in real-time.
- `dlogsize`: Displays the size of log files for all containers, sorted by size.
- `dips`: Lists IP addresses of all running containers in a formatted table.

#### Container Lifecycle Management
- `dstop`: Stops a specific container by name or ID.
- `dstopall`: Stops all running containers on the system.
- `drm`: Removes specified containers from the system.

#### System Cleanup
- `dprunevol`: Removes all unused Docker volumes to free up space.
- `dprunesys`: Performs a complete system cleanup of unused Docker resources.
- `ddelimages`: Removes all Docker images from the system.
- `derase`: Performs a complete cleanup by stopping and removing all containers, images, and volumes.
- `dprune`: Safely removes unused resources while preserving active ones.

### Docker Compose and Traefik

#### Basic Compose Operations
- `dcrun`: Base command for Docker Compose operations using the host-specific compose file.
- `dcup`: Starts all services with automatic rebuilding and orphan container removal.
- `dcdown`: Stops and removes all containers defined in the compose file.
- `dcstop`: Stops services without removing containers.
- `dcstart`: Starts existing containers without recreation.
- `dcrestart`: Restarts all services defined in the compose file.
- `dcrec`: Forces recreation of containers even if their configuration hasn't changed.
- `dcpull`: Updates service images to their latest versions.
- `dclogs`: Shows and follows the last 50 lines of service logs.

#### Traefik Specific
- `traefiklogs`: Monitors the Traefik reverse proxy logs in real-time.

### CrowdSec Security

#### CLI Commands
- `cscli`: Executes CrowdSec CLI commands within the CrowdSec container.
- `csdecisions`: Lists current IP ban decisions made by CrowdSec.
- `csalerts`: Displays security alerts detected by CrowdSec.
- `csinspect`: Provides detailed inspection of specific security alerts.
- `cshubs`: Lists available CrowdSec hubs for security rules.
- `csparsers`: Shows installed log parsers for threat detection.
- `cscollections`: Displays installed collections of security rules.
- `cshubupdate`: Updates the CrowdSec hub index with latest rules.
- `cshubupgrade`: Upgrades all installed hub items to latest versions.
- `csmetrics`: Shows CrowdSec performance and operation metrics.
- `csmachines`: Lists registered CrowdSec machines in the network.
- `csbouncers`: Displays registered bouncers for threat remediation.

#### Service Management
- `csfbstatus`: Checks the status of the CrowdSec firewall bouncer service.
- `csfbstart`: Starts the firewall bouncer service for active protection.
- `csfbstop`: Stops the firewall bouncer service when needed.
- `csfbrestart`: Restarts the firewall bouncer service to apply changes.
- `csbrestart`: Restarts both Traefik bouncer and firewall bouncer services.

#### Log Monitoring
- `tailkern`: Monitors kernel logs for security-related events.
- `tailauth`: Follows authentication logs for access attempts.
- `tailcsfb`: Monitors the CrowdSec firewall bouncer logs.

### System Administration

#### File Navigation
- `cd..`, `..`: Moves up one directory level (two different aliases for the same operation).
- `...`: Moves up two directory levels quickly.
- `.3`: Navigates up three directory levels.
- `.4`: Moves up four directory levels.
- `.5`: Navigates up five directory levels.
- `cdd`: Navigates directly to the Docker folder.
- `up`: Alternative way to move up one directory level.
- `back`: Returns to the previous directory (toggles between last two directories).
- `home`: Navigates to the home directory.

#### File Management
- `ls`: Shows colorized file listing with directories first.
- `ll`: Displays detailed list with human-readable sizes.
- `lt`: Lists files sorted by size with classification.
- `lsr`: Shows files sorted by modification time.
- `mkdir`: Creates parent directories as needed with verbose output.
- `cp`: Performs interactive and verbose copy operations.
- `mv`: Performs interactive and verbose move operations.

#### Storage Management
- `fdisk`: Lists all disk partitions on the system.
- `uuid`: Shows UUID of volumes for identification.
- `mounts`: Shows mounted filesystems in a clean format.
- `dirsize`: Shows directory sizes at depth 1.
- `dirusage`: Displays total disk usage in current directory.
- `diskusage`: Shows total system disk usage.
- `partusage`: Displays partition usage excluding virtual filesystems.
- `space`: Lists directory contents sorted by size.
- `disks`: Lists block devices with details.
- `space-usage`: Shows disk space usage excluding virtual filesystems.
- `inode-usage`: Shows inode usage on mounted filesystems.

#### File Analysis
- `biggest`: Shows the 10 largest files in the current directory.
- `recent`: Displays the 10 most recently modified files.
- `tree`: Shows directory structure in tree format with colors.

### System Monitoring

#### Memory and CPU
- `meminfo`: Shows detailed memory usage information.
- `psmem`: Lists all processes sorted by memory usage.
- `psmem10`: Shows top 10 memory-consuming processes.
- `pscpu`: Lists all processes sorted by CPU usage.
- `pscpu10`: Shows top 10 CPU-consuming processes.
- `cpuinfo`: Displays detailed CPU information.
- `gpumeminfo`: Shows GPU memory information from X.org logs.
- `free`: Displays memory usage in human-readable format.

### Network Management

#### Network Tools
- `ports`: Shows all active ports and their associated processes.
- `showlistening`: Lists all processes listening on ports.
- `ping`: Sends 5 ping packets to test connectivity.
- `myip`: Shows your external IP address.
- `localip`: Shows your primary local IP address.
- `header`: Retrieves HTTP headers from web servers.
- `dnscheck`: Performs quick DNS lookups.
- `connections`: Counts active network connections by state.
- `bandwidth`: Monitors bandwidth usage by process.

### Package Management

#### APT Operations
- `update`: Updates package index files.
- `upgrade`: Updates package index and upgrades all packages.
- `install`: Installs new packages.
- `finstall`: Fixes broken package installations.
- `rinstall`: Reinstalls packages to fix issues.
- `uninstall`: Removes installed packages.
- `search`: Searches for packages in repositories.
- `addkey`: Adds repository keys for package authentication.

### Service Management

#### Systemd Controls
- `ctlreload`: Reloads systemd manager configuration.
- `ctlstart`: Starts systemd services.
- `ctlstop`: Stops systemd services.
- `ctlrestart`: Restarts systemd services.
- `ctlstatus`: Shows status of systemd services.
- `ctlenable`: Enables services to start at boot.
- `ctldisable`: Disables services from starting at boot.
- `ctlactive`: Checks if services are currently active.

#### UFW Firewall
- `ufwenable`: Enables the UFW firewall.
- `ufwdisable`: Disables the UFW firewall.
- `ufwallow`: Allows traffic through specified ports.
- `ufwlimit`: Sets connection limits for ports.
- `ufwlist`: Shows numbered list of UFW rules.
- `ufwdelete`: Removes UFW rules.
- `ufwreload`: Reloads UFW configuration.

### Synology DSM

#### Service Control
- `servicelist`: Lists all Synology services (DSM 6.x only).
- `servicestatus`: Shows status of Synology services.
- `servicestop`: Stops Synology services.
- `servicehstop`: Hard stops services (DSM 6.x only).
- `servicestart`: Starts Synology services.
- `servicehstart`: Hard starts services (DSM 6.x only).
- `servicerestart`: Restarts Synology services.
- `restartdocker`: Restarts the Docker package on Synology.

### File Compression

#### Archive Operations
- `untargz`: Extracts .tar.gz archives preserving ownership.
- `untarbz`: Extracts .tar.bz2 archives preserving ownership.
- `lstargz`: Lists contents of .tar.gz archives.
- `lstarbz`: Lists contents of .tar.bz2 archives.
- `targz`: Creates .tar.gz archives.
- `tarbz`: Creates .tar.bz2 archives.

### Configuration Management

#### Quick Access
- `reload`: Reloads bash configuration from ~/.bashrc.
- `aliases`: Opens the bash aliases file in nano editor.
- `hosts`: Opens the system hosts file in nano editor.
- `bashrc`: Opens the bash configuration file in nano editor.

## Sources
- [Deployrr Docker Aliases](https://github.com/SimpleHomelab/Deployrr/blob/main/includes/docker_aliases)
- [Essential Docker Commands and Time-saving Bash Aliases](https://www.simplehomelab.com/udms-13-docker-and-docker-compose-commands/)
