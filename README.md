[![build status](https://travis-ci.org/AdrianGPrado/ansible-coreos-nginx.svg?branch=master)](https://travis-ci.org/AdrianGPrado/ansible-coreos-nginx.svg?branch=master)

Ansible Role: Nginx
=========

Installs Nginx on CoreOS servers.

This role installs and configures the latest version of Nginx from the Nginx with the Nginx image from Nginx in [DockerHub](https://hub.docker.com/_/nginx/). You can add your personal configuration of servers in `/etc/nginx/conf.d/`, by specifying variables to the ansible playbook, describing the location and options to use for your particular website. There is an example in `nginx_vhost.yml.example`.


Requirements
------------

In order to effectively run ansible, the target machine needs to have a python interpreter. Coreos machines are minimal and do not ship with any version of python. To get around this limitation we can install [pypy](http://pypy.org/), a lightweight python interpreter. The coreos-bootstrap role will install pypy for us and we will update our inventory file to use the installed python interpreter.

    ansible-galaxy install AdrianGPrado.coreos-bootstrap

Dependencies
------------

Boostrap CoreOS with pypy, you can use the boostrap of your preference. Nevertheless I try to keep `AdrianGPrado.coreos-bootstrap` update and running.

    - host: servers
      roles:
        - { role: AdrianGPrado.coreos-bootstrap }

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: AdrianGPrado.coreos-nginx }

Example with self configured nginx vhosts.

    - hosts: servers

      vars_files:
      - secrets.vault.yml

      vars:
        vhosts:
        - default_80:
          fname: port_80.conf
          from: vhosts.j2
          listen: '80 default_server'
          server_name: 'nrgjhub.com'
          error_pages:
            - '404    /404.html'
            - '500 502 503 504  /50x.html'
          locations:
            - location_path: '/'
              location_conf: |
                rewrite        ^ https://$host$request_uri? permanent;
        - default_443:
          fname: 'default_443.conf'
          from: vhosts.j2
          listen: '443 ssl'
          server_name: 'nrgjhub.com'
          error_pages:
            - '404              /404.html'
            - '500 502 503 504  /50x.html'
          locations:
            - location_path: '/'
              location_conf: |
                index index.html index.htm;
                root /usr/share/nginx/html;
          ssl_certs_configuration: |
            ssl on;
            ssl_certificate     {{ nginx_ssl_path }}/{{ ssl_cert_fname }};
            ssl_certificate_key {{ nginx_ssl_path }}/{{ ssl_key_fname }};
          ssl_extra_configuration: |
            ssl_ciphers               "AES128+EECDH:AES128+EDH";
            ssl_stapling              on; # Requires nginx >= 1.3.;
            ssl_protocols             TLSv1 TLSv1.1 TLSv1.2;
            ssl_session_cache         shared:SSL:1m;
            ssl_stapling_verify       on; # Requires nginx => 1.3.;
            resolver_timeout          5m;
            ssl_session_timeout       5m;
            ssl_prefer_server_ciphers on;

      roles:
         - { role: coreos-nginx, nginx_vhosts: '{{ vhosts }}' }

License
-------

MIT / BSD
