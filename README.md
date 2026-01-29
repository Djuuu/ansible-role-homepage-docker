Ansible Role: Homepage-docker
=============================

Install Homepage Docker Compose project.

- https://gethomepage.dev/
- https://github.com/gethomepage/homepage

Requirements
------------

Requires the following to be installed:
- docker
- docker compose

Role Variables
--------------

Common Docker projects variables:

```yaml
# Base directory for Docker projects
docker_projects_path: # /var/apps
```

Available role variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Docker project variables

homepage_project_name: homepage

# Main service additional docker-compose options (ex: cpu_shares, deploy, ...)
homepage_service_additional_options: |
  #ports:
  #  - 3000:3000
```

```yaml
# Homepage project variables

# ghcr.io/gethomepage/homepage image version
homepage_version: latest

# UID container is running as
homepage_puid: "{{ ansible_facts['user_uid'] }}"
# GID container is running as
homepage_pgid: "{{ ansible_facts['user_gid'] }}"

# (optional) For docker integrations
homepage_mount_docker_socket: false

# Allowed hosts homepage can be exposed on (with port if necessary) 
homepage_allowed_hosts: []
#  - homepage.lan:3000
#  - homepage.mydomain.net

# Environment secrets
# https://gethomepage.dev/installation/docker/#using-environment-secrets
homepage_env:
#  HOMEPAGE_VAR_SOME_EXAMPLE: some-example
#  HOMEPAGE_VAR_OTHER_EXAMPLE: other-example
```

Config files
------------

Files in the following locations will be copied in the project's main directory:

- `config/homepage/{{ inventory_hostname }}/config/*`  
  Available configuration files:  
  - `bookmarks.yaml`
  - `custom.css`
  - `custom.js`
  - `docker.yaml`
  - `kubernetes.yaml`
  - `services.yaml`
  - `settings.yaml`
  - `widgets.yaml`

- `config/homepage/{{ inventory_hostname }}/images/*`

See https://gethomepage.dev/configs/ for details.

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Some variables allow integration with:
- [djuuu.traefik_docker](https://github.com/Djuuu/ansible-role-traefik-docker)

Example Playbook
----------------

```yaml
- hosts: all
  gather_facts: true
  gather_subset:
    - "!all"
    - "!min"
    - user_id

  roles:
    - djuuu.homepage_docker
```

License
-------

Beerware License
