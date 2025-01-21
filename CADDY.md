# Caddy Proxy

```
caddy:
  image: caddy:latest
  restart: unless-stopped
  ports:
    - "80:80"
    - "443:443"
  volumes:
    - caddy_data:/data
    - ${DATA_FOLDER}/caddy_config:/config
    - ${DATA_FOLDER}/caddy_config/Caddyfile:/etc/caddy/Caddyfile
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
