# Ansible Collection - mkbrechtel.srvapp

Ansible collection for deploying server applications.

## Installation

Add to your `requirements.yml`:

```yaml
collections:
  - name: https://github.com/mkbrechtel/srvapp.git
    type: git
    version: main
```

Then install with:

```bash
ansible-galaxy collection install -r requirements.yml
```

## Roles

- **traefik** - Reverse proxy with automatic HTTPS using the [Host-wide Traefik Container Pattern](https://patterns.how/operation/deployment/host-traefik.md)

## License

Apache-2.0
