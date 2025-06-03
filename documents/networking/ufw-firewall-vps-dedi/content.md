# Deployrr & UFW Firewall

One of the biggest pain points when using Docker on a VPS/Dedicated Server is dealing with firewall issues. In almost all cases of remotely affordable cloud VPS or Dedicated Servers, you are given a machine with a public IP and no out of operating system firewall rules (think firewall that is applied to your servers that you use outside of the machine itself and inside the company web interface. Oracle Cloud & AWS are good examples of this, typically if you look up anything involving opening ports for these then your first step is opening ports inside your cloud control panel).

### You may see strange behavior like the following things visually
- Your self-hosted apps show a valid SSL certificate inside your browser
- The self-hosted apps themselves aren't able to make a connection to the internet so you see functions inside the application erroring out or not working

and either inside the container logs or traefik logs you will see errors mentioning "Self-signed certificate", "TLS errors", "MAC record errors", similar stuff.

### Example from traefik logs

```
2025-05-23T16:10:11-04:00 DBG github.com/traefik/traefik/v3/pkg/tls/tlsmanager.go:228 > Serving default certificate for request: "connect.somethirdpartyAPI.com" 2025-05-23T16:10:11-04:00 DBG log/log.go:245 > http: TLS handshake error from 192.168.91.1:42832: local error: tls: bad record MAC
```

### Example from inside a container's shell trying to wget or curl google.com

```
/ # curl -Iv https://google.com * Host google.com:443 was resolved. * IPv6: 2607:f8b0:4009:808::200e * IPv4: 142.250.191.110 * Trying [2607:f8b0:4009:808::200e]:443... * Immediate connect fail for 2607:f8b0:4009:808::200e: Network unreachable * Trying 142.250.191.110:443... * ALPN: curl offers h2,http/1.1 * TLSv1.3 (OUT), TLS handshake, Client hello (1): * CAfile: /etc/ssl/cert.pem * CApath: /etc/ssl/certs * TLSv1.3 (IN), TLS handshake, Server hello (2): * TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8): * TLSv1.3 (IN), TLS handshake, Certificate (11): * TLSv1.3 (OUT), TLS alert, unknown CA (560): * SSL certificate problem: self-signed certificate * closing connection #0 curl: (60) SSL certificate problem: self-signed certificate More details here: https://curl.se/docs/sslcerts.html curl failed to verify the legitimacy of the server and therefore could not establish a secure connection to it. To learn more about this situation and how to fix it, please visit the webpage mentioned above.
```

Every Google or LLM result will have you thinking this is to do with the way the application containers are being built or some sort of lack of Root CA certificates either on your system or inside Docker containers.

