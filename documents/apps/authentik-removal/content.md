# Authentik Removal

This guide explains how to completely remove Authentik from your Deployrr installation, including all its dependencies and components.

## Known Limitations

When using Deployrr's "Full Delete" feature for Authentik:
- Database operations are not handled by Deployrr
- Only the main service (Authentik Server) is removed automatically
- Supporting services (Authentik Worker, PostgreSQL, Redis) must be removed manually using the Stack Manager

## Complete Removal Steps

To fully remove Authentik and all its components, "Full Delete" the following services in Stack Manager:

   - authentik (main service, aka Authentik Server)
   - authentik-worker (supporting service, aka Authentik Worker)
   - PostgreSQL (only if no other services are using it)
   - Redis (only if no other services are using it)

## Important Notes

- Wait for each removal operation to complete before proceeding to the next
- Backup any important data before starting the removal process
- Verify that PostgreSQL and Redis are not used by other services before removing them

## Sources

- [Discord Thread Discussion](https://discord.com/channels/974306760171073556/1339604236765237299)
