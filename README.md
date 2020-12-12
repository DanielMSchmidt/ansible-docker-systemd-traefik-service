# docker-systemd-traefik-service

Systemd managed docker containers, exposed by traefik, enabled for zero-downtime deployments.

```yaml
- name: Start WebApp
  include_role:
    name: docker-systemd-traefik-service
  vars:
    container_name: myapp
    container_image: myapp:latest
    container_links: ["mysql"]
    container_volumes:
      - "/data/uploads:/data/uploads"
    container_ports:
      - "3000:3000"
    container_env:
      MYSQL_ROOT_PASSWORD: "{{ mysql_root_pw }}"
    container_labels:
      - traefik.http.middlewares.myapp-stripprefix.stripprefix.prefixes="{{ subpath }}"
    traefik_router_labels:
      - entrypoints=websecure
      - tls=true
      - tls.certresolver=myapp
      - rule="Host(`{{ host }}`) && PathPrefix(`{{ subpath }}`)"
      - middlewares=myapp-stripprefix@docker
```

## Requirements

[mhutter.docker-systemd-service](https://github.com/mhutter/ansible-docker-systemd-service) must be installed.

## Role Variables

The same as [mhutter.docker-systemd-service](https://github.com/mhutter/ansible-docker-systemd-service), plus the listed extra ones:

- `traefik_router_labels`: List of labels being prefixed for traefik with the `container_name`. E.g. `entrypoints=websecure` becomes the label `traefik.http.routers.myapp.entrypoints=websecure` (or during zero-downtime deployment `traefik.http.routers.idadwh-tmp.entrypoints=websecure`)

## License

BSD
