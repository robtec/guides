# Caddy Proxy

Network
```
$ docker network create -d bridge internal-apps
```

`docker-compose.yml`

```
services:
  caddy:
    image: caddy:${VERSION}
    restart: unless-stopped
    container_name: caddy
    networks
      - internal-apps
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - caddy-data:/data
      - caddy-config:/config
      - ./Caddyfile:/etc/caddy/Caddyfile:ro

volumes:
  caddy-data:
  caddy-config:
```

`Caddyfile`

```
example.com {
  root * /var/www/site
  file_server
}

app.example.com {
  reverse_proxy app:8080 {
    flush_interval -1
  }
}
```

Examples - https://caddyserver.com/docs/caddyfile/directives/reverse_proxy#examples
