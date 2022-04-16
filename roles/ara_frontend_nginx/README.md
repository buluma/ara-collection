# Ansible role: ara_frontend_nginx

A role for deploying a minimal installation of nginx for ara-api and ara-web.

This role is not meant to be used on it's own, it should be included by the
``ara_api`` and ``ara_web`` roles in order to have the necessary variables
available.

It is currently tested and supported against Ubuntu 18.04 and Fedora 29.

## Role Variables

- ``ara_api_frontend_vhost``: Path to a custom nginx vhost configuration file for ara-api.
- ``ara_web_frontend_vhost``: Path to a custom nginx vhost configuration file for ara-web.

## Example playbooks

Install ARA and set up the API to be served by nginx with a custom vhost configuration
in front of gunicorn:

```yaml
# The API will be reachable at http://api.ara.example.org
# The web interface will be reachable at http://web.ara.example.org
# The web interface will be set up to query api.ara.example.org.
- name: Deploy ARA API server and web interface
  hosts: all
  gather_facts: yes
  vars:
    # ara_api
    ara_api_frontend_server: nginx
    ara_api_wsgi_server: gunicorn
    ara_api_fqdn: api.ara.example.org
    ara_api_allowed_hosts:
      - api.ara.example.org
    ara_api_frontend_vhost: custom_api_vhost.conf.j2
    # ara_web
    ara_web_fqdn: web.ara.example.org
    ara_web_api_endpoint: "http://api.ara.example.org"
    ara_web_frontend_server: nginx
    ara_web_frontend_vhost: custom_web_vhost.conf.j2
  roles:
    - ara_api
    - ara_web
```
