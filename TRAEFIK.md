# Traefik config for Docker Compose

```
version: "3.9"

services:

  traefik:
    image: traefik:v2.9
    restart: always
    container_name: traefik
    command:
      - "--log.level=DEBUG"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.web.http.redirections.entryPoint.to=https"
      - "--entrypoints.web.http.redirections.entryPoint.scheme=https"
      - "--entrypoints.websecure.address=:443"
      - "--certificatesresolvers.myresolver.acme.tlschallenge=true"
      - "--certificatesresolvers.myresolver.acme.email=${LETSENCRYPT_EMAIL}"
      - "--certificatesresolvers.myresolver.acme.storage=/letsencrypt/acme.json"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
      - "/etc/letsencrypt:/letsencrypt"
    depends_on:
      - database
    logging:
      options:
        max-size: "10m"
        max-file: "3"
    ports:
      - 80:80
      - 443:443
```

Labels for load balanced containers
```
    labels:
      traefik.enable: true
      traefik.http.routers.rocketchat.rule: Host(`${PRIMARY_DOMAIN}`)
      traefik.http.services.rocketchat.loadbalancer.server.port: 3000
      traefik.http.routers.rocketchat.entrypoints: websecure
      traefik.http.routers.rocketchat.tls.certresolver: myresolver
```
