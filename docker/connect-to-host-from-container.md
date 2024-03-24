# Connect to Host from Container

## Summary & Effect

Add "host-gateway" mapping to extra\_hosts:

```yaml
extra_hosts:
  - "host.docker.internal:host-gateway"
```

An application inside a container can have a network connect to the host application with 'host.docker.internal' which is mapped to 'host-gateway'

## Example

### docker-compose.yaml

```yaml
services:
  nginx:
    image: nginx
    volumes:
      - "./nginx.conf:/etc/nginx/nginx.conf:ro"
    extra_hosts:
      - "host.docker.internal:host-gateway"
```

### nginx.conf

```nginx
http {
    ...
    server {
        ...
        
        location / {
            proxy_pass  http://host.docker.internal:3000;
        }

        location /api {
            proxy_pass  http://host.docker.internal:8080;
        }
        
        ...
    }
}
```
