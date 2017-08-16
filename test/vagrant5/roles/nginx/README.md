Nginx
=========

Demo setup of nginx with reverse proxy configuration

Role Variables
--------------

Variables defined under `vars/main.yml`
- nginx_sites_available_dir: /etc/nginx/sites-available
- nginx_sites_enabled_dir: /etc/nginx/sites-enabled

Variables required to be provided
nginx_reverse_proxies:
- config_name: `<name_for_your_config>`

  backend_name: `<name_for_your_server>`

  balancer_type: `<e.g. least_conn>`

  backend_url: `<hostname_to_send_requests>`

  backends:
  - `<host_name_to_send_requests>`:`<port_listening_for_requests>`


Dependencies
------------
None

Example Playbook
----------------

``` yaml
---
- host: all
  vars:
    ghost_link:
      host: "127.0.0.1"
      port: "2368"
    nginx_reverse_proxies:
      - config_name: ghostproxy
        backend_name: ghost
        balancer_type: least_conn
        backend_url: "{{ghost_link.host}}"
        backends:
          - "{{ghost_link.host}}:{{ghost_link.port}}"

roles:
  - nginx

```



License
-------

MIT

Author Information
------------------

Mohit Sharma
