# Traefik Logs

Traefik is a powerful reverse proxy that provides detailed logging capabilities. This guide explains how to effectively monitor and manage Traefik logs in your Docker environment.

## Log File Location and Configuration

Traefik maintains two separate log files:

1. `traefik.log` - Contains general Traefik operational logs (errors and warnings)
2. `access.log` - Contains access logs for requests handled by Traefik

These files are located in:
```
/home/user/docker/logs/hostname/traefik/
```

The logging configuration is defined in the Traefik Docker Compose file through CLI arguments:

```yaml
- --log=true
- --log.filePath=/logs/traefik.log
- --log.level=DEBUG # (Default: error) DEBUG, INFO, WARN, ERROR, FATAL, PANIC
- --accessLog=true
- --accessLog.filePath=/logs/access.log
- --accessLog.bufferingSize=100 # Configuring a buffer of 100 lines
- --accessLog.filters.statusCodes=204-299,400-499,500-599
```

## Why File-Based Logging?

Traefik is configured to write logs to files instead of standard output for several key benefits:

1. **Advanced Filtering**: 
   - Access logs can be filtered by status codes
   - Helps reduce noise and focus on relevant events

2. **Log Persistence**: 
   - Logs persist between container restarts
   - Maintains historical data for troubleshooting

3. **Integration Capabilities**: 
   - Easier integration with log management systems
   - Ability pass the logs to CrowdSec for real-time threat detection
   - Compatible with log rotation tools

## Viewing Traefik Logs

### ⚠️ Important Warning
The commands `docker compose logs traefik` or `docker logs traefik` will show empty results. This is because Traefik is configured to write logs directly to files instead of standard output.

### Standard Commands
To view Traefik logs in real-time:

```bash
# View Traefik operational logs
tail -f /home/user/docker/logs/hostname/traefik/traefik.log

# View access logs
tail -f /home/user/docker/logs/hostname/traefik/access.log
```

### Using Bash Aliases
If you're using Anand's bash aliases, you can use the simplified command:

```bash
traefiklogs
```

## Viewing Traefik Logs in Dozzle

If you are interested in viewing the logs in a more user-friendly way, you can use Dozzle. To pass Traefik logs to Dozzle, you can setup Traefik Error Logs and Traefik Access Logs apps from Deployrr Apps menu. 

## Log Levels

Traefik supports the following log levels (in order of verbosity):

| Level | Description |
|-------|-------------|
| DEBUG | Most verbose, useful during setup and troubleshooting |
| INFO | General operational logs |
| WARN | Warning messages |
| ERROR | Error messages |
| FATAL | Fatal errors that cause Traefik to stop |
| PANIC | Severe errors causing immediate termination |

### Best Practices for Log Levels

1. **During Setup/Troubleshooting**:
   - Use DEBUG level for maximum visibility
   - Helps identify configuration issues and certificate problems

2. **Production Environment**:
   - Switch to INFO or WARN level
   - Reduces log volume while maintaining important information

To change the log level, modify the `--log.level` argument in your Traefik Docker Compose file:

```yaml
--log.level=INFO  # Change from DEBUG to INFO after setup
```

## Tips for Log Management

1. **Initial Setup**:
   - Keep log level at DEBUG
   - Monitor both traefik.log and access.log
   - Pay attention to LetsEncrypt certificate-related messages

2. **Access Log Optimization**:
   - Adjust status code filters to reduce noise:
   ```yaml
   --accessLog.filters.statusCodes=204-299,400-499,500-599
   ```
   - This captures successful responses, client errors, and server errors

3. **Log Buffering**:
   - Remember logs are buffered (100 lines) before disk write
   - Expect slight delay in seeing latest entries
   - Increase or decrease buffer size based on needs:
   ```yaml
   --accessLog.bufferingSize=100  # Adjust as needed
   ```

4. **Log Rotation**:
   - Implement log rotation to manage disk space
   - Consider using external log management solutions for long-term storage

## Common Troubleshooting Scenarios

1. **Empty Docker Logs**:
   - If `docker logs traefik` shows nothing, this is normal
   - Use file-based log viewing commands instead

2. **Missing Log Files**:
   - Ensure proper volume mapping in Docker Compose
   - Check directory permissions
   - Verify log paths in Traefik configuration

3. **High Log Volume**:
   - Adjust log level to reduce verbosity
   - Fine-tune access log filters
   - Consider implementing log rotation



