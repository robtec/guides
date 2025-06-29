# Traefik config for Docker Compose

* https://doc.traefik.io/traefik/reference/dynamic-configuration/docker/

* https://github.com/bluepuma77/traefik-best-practice/

# v3
```
services:
  traefik:
    image: traefik:v3.3.4
    container_name: traefik
    ports:
      - 80:80
      - 443:443
    restart: always
    labels:
      - traefik.enable=true
      - traefik.docker.network=traefik-public
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - traefik-public-certificates:/certificates
    command:
      - --providers.docker
      - --providers.docker.exposedbydefault=false
      - --entrypoints.web.address=:80
      - --entrypoints.web.http.redirections.entryPoint.scheme=https
      - --entrypoints.web.http.redirections.entryPoint.to=websecure
      - --entrypoints.websecure.address=:443
      - --certificatesresolvers.le.acme.storage=/certificates/acme.json
      - --certificatesresolvers.le.acme.email=${LE_EMAIL}
      - --certificatesresolvers.le.acme.tlschallenge=true
      - --accesslog
      - --log
      - --api
      - --log.level=DEBUG
    networks:
      - traefik-public

volumes:
  traefik-public-certificates:

networks:
  traefik-public:
```

# labels
```
labels:
  - traefik.enable=true
  - traefik.docker.network=traefik-public
  - traefik.constraint-label=traefik-public
  - traefik.http.routers.XXXXX.rule=Host(`${DOMAIN}`)
  - traefik.http.routers.XXXXX.entrypoints=https
  - traefik.http.routers.XXXXX.tls=true
  - traefik.http.routers.XXXXX.tls.certresolver=le
  - traefik.http.services.XXXXX.loadbalancer.server.port=8080
```

# v2
```
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
      - "--entrypoints.web.http.redirections.entryPoint.to=websecure"
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
      traefik.http.routers.XXXXXapp.rule: Host(`${PRIMARY_DOMAIN}`)
      traefik.http.services.XXXXXapp.loadbalancer.server.port: 3000
      traefik.http.routers.XXXXXapp.entrypoints: websecure
      traefik.http.routers.XXXXXapp.tls.certresolver: myresolver
```

.env
```
PRIMARY_DOMAIN=
LETSENCRYPT_EMAIL=
```
