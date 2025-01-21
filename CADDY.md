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
n8n.domain.dev {
    reverse_proxy n8n:5678 {
      flush_interval -1
    }
}
```
