

# traefik.yml

```yml
global:
  checkNewVersion: true
  sendAnonymousUsage: false  # true by default

# (Optional) Log information
# ---
# log:
#  level: ERROR  # DEBUG, INFO, WARNING, ERROR, CRITICAL
#   format: common  # common, json, logfmt
#   filePath: /var/log/traefik/traefik.log

# (Optional) Accesslog
# ---
# accesslog:
  # format: common  # common, json, logfmt
  # filePath: /var/log/traefik/access.log

# (Optional) Enable API and Dashboard
# ---
api:
 dashboard: true  # true by default
 insecure: true  # Don't do this in production!

# Entry Points configuration
# ---
entryPoints:
  web:
    address: :80
    # (Optional) Redirect to HTTPS
    # ---
    http:
      redirections:
        entryPoint:
          to: websecure
          scheme: https

  websecure:
    address: :443

# Configure your CertificateResolver here...
# ---
certificatesResolvers:
  myresolver:
    acme:
      email: namecheap@callum.mozmail.com
      storage: /etc/traefik/certs/acme.json
      caServer: "https://acme-staging-v02.api.letsencrypt.org/directory"
      httpChallenge:
        entryPoint: web

# (Optional) Overwrite Default Certificates
tls:
  stores:
    default:
      defaultCertificate:
        certFile: /etc/traefik/certs/cert.pem
        keyFile: /etc/traefik/certs/cert-key.pem
# (Optional) Disable TLS version 1.0 and 1.1
#   options:
#     default:
#       minVersion: VersionTLS12

providers:
  docker:
    exposedByDefault: false  # Default is true
  file:
    # watch for dynamic configuration changes
    directory: /etc/traefik
    watch: true

```


# Docker labels

```yml
  nginx:
    image: nginx:latest
    container_name: nginx_webserver
    labels:
      - "traefik.enable=true" # Enable traefik to handle this container
      - "traefik.http.routers.nginx.rule=Host(`callumwellard.co.uk`)"
      - "traefik.http.routers.nginx.entrypoints=websecure"
      - "traefik.http.routers.nginx.tls.certresolver=myresolver" # Set up SSL certification
      - "traefik.http.services.nginx.loadbalancer.server.port=80"  # Set the port of NGINX
```