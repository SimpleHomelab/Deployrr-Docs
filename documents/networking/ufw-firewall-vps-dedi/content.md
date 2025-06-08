# Deployrr & UFW Firewall

One of the biggest pain points when using Docker in any capacity on a VPS/Dedicated Server is dealing with firewall issues. In almost all cases of remotely affordable cloud VPS or Dedicated Servers, you are given a machine with a public IP and no out of operating system firewall rules (think firewall that is applied to your servers that you use outside of the machine itself and inside the company web interface. Oracle Cloud & AWS are good examples of this, typically if you look up anything involving opening ports for these then your first step is opening ports inside your cloud control panel).

### You may see strange behavior like the following things visually
- Your self-hosted apps show a valid SSL certificate inside your browser
- The self-hosted apps themselves aren't able to make a connection to the internet so you see functions inside the application erroring out or not working

and either inside the container logs or traefik logs you will see errors mentioning "Self-signed certificate", "TLS errors", "MAC record errors", "Serving default certificate", or something *along those lines*. 

### Example from traefik logs

```
2025-05-23T16:10:11-04:00 DBG github.com/traefik/traefik/v3/pkg/tls/tlsmanager.go:228 > Serving default certificate for request: "connect.somethirdpartyAPI.com" 2025-05-23T16:10:11-04:00 DBG log/log.go:245 > http: TLS handshake error from 192.168.91.1:42832: local error: tls: bad record MAC
```

### Example from inside a container's shell trying to wget or curl google.com

```
/ # curl -Iv https://google.com * Host google.com:443 was resolved. * IPv6: 2607:f8b0:4009:808::200e * IPv4: 142.250.191.110 * Trying [2607:f8b0:4009:808::200e]:443... * Immediate connect fail for 2607:f8b0:4009:808::200e: Network unreachable * Trying 142.250.191.110:443... * ALPN: curl offers h2,http/1.1 * TLSv1.3 (OUT), TLS handshake, Client hello (1): * CAfile: /etc/ssl/cert.pem * CApath: /etc/ssl/certs * TLSv1.3 (IN), TLS handshake, Server hello (2): * TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8): * TLSv1.3 (IN), TLS handshake, Certificate (11): * TLSv1.3 (OUT), TLS alert, unknown CA (560): * SSL certificate problem: self-signed certificate * closing connection #0 curl: (60) SSL certificate problem: self-signed certificate More details here: https://curl.se/docs/sslcerts.html curl failed to verify the legitimacy of the server and therefore could not establish a secure connection to it. To learn more about this situation and how to fix it, please visit the webpage mentioned above.
```

Every Google or LLM result will have you thinking this is to do with the way the application containers are being built or some sort of lack of Root CA certificates either on your system or inside Docker containers.

### Best Practices / Troubleshooting steps

If everything I've described about the "environment" so far sounds similar to yours e.g.  VPS with a public IP, no external firewall control panel (if you were hosting at home your "external firewall" in this case would be your router/modem's portforwarding page) then go on to see how I did it and what worked for me.

**TIP:** Firewall works in layers depending on your environment. Say you were hosting inside your home on a home-server, you need to go down your layers and work your way backwards. For example, if you're running Proxmox, then make sure you setup the firewall correct inside the virtual machine itself, then do the same rules as the virtual machine's firewall inside the Proxmox Web Control Panel in the Firewall section, then do the same rules inside your modem/router. 

I'm not telling you to do this, I'm just trying to get you to understand the layered approach and how things all come together. Personally, I don't think all of that would be necessary. If I was in that situation then I would probably just leave the VM operating system firewall & Proxmox firewall's wide open then only do my firewall rules inside my modem/router as the only and "final word" of the firewall. This is a topic subject to a lot of personal opinion, some people say more layers is better, or you should have multi-tiering because of security or what not. It's probably ok however you like but if you're having problems best to keep it as simple as possible. Keep in mind, I wrote this for my own environment which is a VPS or Dedi with a public IP and no external firewall.

## Undo firewall modifications you've made

If you have touched or messed with UFW, IPTables, or any other firewall files/packages **inside** your virtual machine's operating system you need to undo them and get them to the default state. This would also include any modifications you've made to /etc/ufw/before.rules or /etc/ufw/after.rules.


## Setup private networking so you get a "LAN" IP

Look at your VM providers documentation for enabling private networking. This will involve probably flipping some sort of switch inside your cloud control panel and then following your provider specific instructions for modifying or adding an additional eth interface through /etc/network/interfaces or "netplan".

## Why?

Because, if you're using Deployrr in Hybrid mode (so you can set some apps to only be accessible through Tailscale/LAN, and also be able to set some apps to the entire internet) you need to have a "LAN" IP so you can differentiate the connections. If you remember looking through the Deployrr app itself in the setup menus or the video's with Deployrr itself, it says in hybrid mode or shows in the video that you need to do some portforwarding of local and remote rules to make 444 go to 443 and 81 go to 80. In this case, we don't have your typical firewall control panel.

For my provider, on Ubuntu, to add private networking I needed to create the file /etc/netplan/90-private.yaml and fill it with this, save, and down, then re-up the network services on the virtual machine.

```
network:
    ethernets:
        enp2s0:
            addresses:
            - 10.0.0.2/24
            match:
                macaddress: 00:22:ZZ:CC:BB:AA
    version: 2
```

Don't just copy this, each provider might have a different interface name, allowed IPs for use with private networking, and a MAC address you need to use depending on **their** setup.


## Then

- Inside the Deployrr app, set your "server IP" or "local IP" to the IP you just assigned, in my case my private networking IP is 10.0.0.2.
- Install UFW-Docker through the Deployrr app.

Do the basic UFW setup which is:
- `sudo ufw default deny incoming` - Deny all connections by default
- `sudo ufw default allow outgoing`Allow all outgoing connections by default
- `sudo ufw allow 22/tcp`- Allow SSH so you **DONT** get locked out, if you changed your SSH port from the default (which you **DEFINITELY** already should have, on top of denying authentication with a password and allowing it only with a SSH key), then you need to change the port from 22 to whatever you set your port as.
- `sudo ufw allow 80/tcp`
- `sudo ufw allow 443/tcp`
- `sudo ufw route allow proto tcp from any to any port 443`
- `sudo ufw route allow proto tcp from any to any port 80`
- `sudo ufw allow in on tailscale0`

Reboot or restart. This should be the achieved effect.

- Apps set to LAN will be accessible through http://YOURTAILNET.domain:port
- Apps set to external will be accessible through your subdomain set through Deployrr traefikify menu
- Apps set to LAN **WONT** be accessible through the public internet


### Other things I learned or tried along the way

Remember when I mentioned the Deployrr setup or Deployrr YouTube videos showing local and remote portforwarding of 444 to 443 and 81 to 80? I actually had previously modified /etc/ufw/before.rules with snippets to do exactly that same "forwarding" as described by the text inside the app and video. 

I used these rules **(WHICH YOU SHOULDNT)**

```
# Forward port 80 to port 81 on the same machine (for Traefik)
#-A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 81

# Forward port 443 to port 444 on the same machine (for Traefik)
#-A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 444
```

And it actually worked for a while, but somewhere along upgrading packages on the system, or Deployrr upgrades itself, and a couple reboots. It stopped working and led me down 12-16 hours of my own personal hell. Hopefully you can avoid these troubles with this guide.


### What's left?

At this current moment, apps set to internal/LAN only access inside Deployrr can only be accessed through http:// and the port number. I'd like for internal/LAN to be accessed with a SSL certificate and without a port number. It may be possible by setting the public A record of the app's subdomain to the private Tailscale IP of the system, but I wasn't able to get it working and haven't spent longer than 10 minutes on trying.