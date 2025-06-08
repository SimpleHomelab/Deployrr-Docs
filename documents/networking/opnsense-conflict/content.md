# OPNsense Conflict with ACME Certificate Validation

There is a known issue where users running OPNsense as their firewall/router may experience problems with Deployrr's ACME certificate validation (DNS challenge) through Traefik. This issue manifests as an error message stating "ERROR unable to obtain ACME certificate" during the Traefik staging phase.

## Symptoms

- Error message: "ERROR unable to obtain ACME certificate"
- Occurs during Traefik staging phase
- Persists even with proper port forwarding and Cloudflare configuration
   - CNAME for traefik pointing to domain
   - CNAME wildcard record (*.domain)
   - A record for domain pointing to public IP
- Issue occurs even with Zenarmor disabled

## Current Status

This is an ongoing issue that appears to be related to how OPNsense handles DNS requests and how they interact with Deployrr's DNS validation process. The exact root cause is still under investigation.

## Temporary Workaround

While a permanent solution is being developed, users can consider using [Tailscale](https://tailscale.com/) as a temporary workaround to access their services.

## Future Updates

This documentation will be updated as more information becomes available and when a permanent solution is found. T.

## Sources

- Discord Thread: https://discord.com/channels/974306760171073556/1362110724666556597
