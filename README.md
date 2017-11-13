httpd
=========

Provides httpd for your system.

Requirements
------------

Access to a repository containing packages, likely on the internet.

Role Variables
--------------

None known.

Dependencies
------------

- robertdebock.bootstrap

Example Playbook
----------------

```
- hosts: servers

  roles:
    - robertdebock.httpd

  tasks:
    - name: place content
      copy:
        src: files/index.html
        dest: /var/www/html/index.html
```

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
