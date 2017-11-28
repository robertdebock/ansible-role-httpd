ansible-role-httpd
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-httpd.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-httpd)

Provides Apache httpd for your system.

Requirements
------------

Access to a repository containing packages, likely on the internet.

Role Variables
--------------

None known.

Dependencies
------------

- robertdebock.ansible-role-bootstrap

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Example Playbook
----------------

```
- hosts: servers

  roles:
    - robertdebock.ansible-role-httpd

  tasks:
    - name: place content
      copy:
        src: files/index.html
        dest: /var/www/html/index.html
```

Install this role using `galaxy install robertdebock.ansible-role-httpd`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
