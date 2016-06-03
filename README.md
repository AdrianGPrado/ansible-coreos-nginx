Ansible Role: Nginx
=========

Installs Nginx on CoreOS servers.

This role installs and configures the latest version of Nginx from the Nginx with the Nginx image from Nginx in [DockerHub](https://hub.docker.com/_/nginx/). You can add your personal configuration of servers in `/etc/nginx/conf.d/`, by specifying variables to the ansible playbook, describing the location and options to use for your particular website. There is an example in `nginx_vhost.yml.example`.


Requirements
------------

In order to effectively run ansible, the target machine needs to have a python interpreter. Coreos machines are minimal and do not ship with any version of python. To get around this limitation we can install [pypy](http://pypy.org/), a lightweight python interpreter. The coreos-bootstrap role will install pypy for us and we will update our inventory file to use the installed python interpreter.

```
ansible-galaxy install AdrianGPrado.coreos-bootstrap
```

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: AdrianGPrado.coreos-nginx }

License
-------

MIT / BSD
