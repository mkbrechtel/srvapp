# Traefik Role

This role deploys Traefik as a reverse proxy with automatic HTTPS using the [Host-wide Traefik Container Pattern](https://patterns.how/operation/deployment/host-traefik.md).

## Pattern Implementation

The pattern provides centralized HTTPS termination and routing for services on a single host without requiring Docker socket access. Key aspects:

- **Shared Docker proxy network** - Services connect to the `proxy` network to be reachable by Traefik
- **File-based configuration** - Per-service routing rules in `conf.d/` directory
- **Automatic certificate management** - Let's Encrypt integration with HTTP to HTTPS redirection

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `traefik_config_directory` | `/etc/traefik` | Configuration directory |
| `traefik_data_directory` | `/var/lib/traefik` | Data directory (ACME certificates) |
| `traefik_docker_network` | `proxy` | Docker network for service communication |
| `traefik_acme_email` | `""` | Email for Let's Encrypt (required for HTTPS) |
| `traefik_image` | `traefik:v3.2` | Traefik container image |

A symlink from `/srv/traefik` to the config directory is created for backwards compatibility with the original pattern.

## Usage

Include the role in your playbook:

```yaml
- hosts: servers
  roles:
    - role: mkbrechtel.srvapp.traefik
      vars:
        traefik_acme_email: admin@example.com
```

## Adding Services

Create routing configuration files in `{{ traefik_config_directory }}/conf.d/{hostname}.yaml`:

```yaml
http:
  routers:
    myservice:
      entrypoints: websecure
      rule: Host(`service.example.com`)
      service: myservice
      tls:
        certResolver: letsencrypt
  services:
    myservice:
      loadBalancer:
        servers:
          - url: http://container_name:port
```

Services must connect to the `proxy` network in their docker-compose.yaml:

```yaml
networks:
  proxy:
    external: true
```

## License

Apache-2.0
