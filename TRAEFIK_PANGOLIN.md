
```
services:

  traefik:
    image: traefik:v3.3.4
    container_name: traefik
    network_mode: service:gerbil
    restart: always
    volumes:
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
      - ./config/traefik:/etc/traefik:ro
      - ./config/letsencrypt:/letsencrypt
    command:
      - --providers.file.filename=/etc/traefik/dynamic_config.yml
      - --providers.http.endpoint=http://pangolin:3001/api/v1/traefik-config
      - --entrypoints.web.address=:80
      - --entrypoints.websecure.address=:443
      - --entrypoints.websecure.transport.respondingtimeouts.readtimeout=30m
      - --serverstransport.insecureskipverify=true
      - --certificatesresolvers.letsencrypt.acme.storage=/letsencrypt/acme.json
      - --certificatesresolvers.letsencrypt.acme.email=${LE_EMAIL}
      - --certificatesresolvers.letsencrypt.acme.tlschallenge=true
      - --api.insecure=true
      - --api.dashboard=true
      - --experimental.plugins.badger.modulename=github.com/fosrl/badger
      - --experimental.plugins.badger.version=v1.2.0
      - --log.level=INFO
      - --log.format=common

  pangolin:
    image: fosrl/pangolin:1.5.1
    container_name: pangolin
    restart: unless-stopped
    volumes:
      - ./config:/app/config
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3001/api/v1/"]
      interval: "3s"
      timeout: "3s"
      retries: 15

  gerbil:
    image: fosrl/gerbil:1.0.0
    container_name: gerbil
    restart: unless-stopped
    depends_on:
      pangolin:
        condition: service_healthy
    command:
      - --reachableAt=http://gerbil:3003
      - --generateAndSaveKeyTo=/var/config/key
      - --remoteConfig=http://pangolin:3001/api/v1/gerbil/get-config
      - --reportBandwidthTo=http://pangolin:3001/api/v1/gerbil/receive-bandwidth
    volumes:
      - ./config/:/var/config
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    ports:
      - 51820:51820/udp
      - 443:443
      - 80:80

networks:
  default:
    driver: bridge
    name: pangolin
```
