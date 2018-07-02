httpd
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-httpd.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-httpd)

Provides Apache httpd for your system.

Context
--------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/httpd.png "Dependency")

Requirements
------------

Access to a repository containing packages, likely on the internet.

Role Variables
--------------

- httpd_applications: a list of applications that will (reversed) proxy to the "backend_url". (See example for more details.) By default this varialbe is not set.
- httpd_servername: The hostname to redirect to. Defaults to "{{ ansible_fqdn }}".
- httpd_port: The plain text TCP port to listen on. Defaults to 80.
- httpd_ssl_servername: The Common Name (CN) to sign ssl certificates with. Defaults to "{{ ansible_fqdn }}".
- httpd_ssl_port: The encrypted TCP port to listen on. Defaults to 443.

Dependencies
------------

This role can be used to prepare your system.

- [robertdebock.bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap)
- [robertdebock.buildtools](https://travis-ci.org/robertdebock/ansible-role-buildtools)
- [robertdebock.epel](https://travis-ci.org/robertdebock/ansible-role-epel)
- [robertdebock.httpd](https://travis-ci.org/robertdebock/ansible-role-httpd)
- [robertdebock.scl](https://travis-ci.org/robertdebock/ansible-role-scl)
- [robertdebock.python-pip](https://travis-ci.org/robertdebock/ansible-role-python-pip)
- [robertdebock.update](https://travis-ci.org/robertdebock/ansible-role-updatebootstrap)

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.3|ansible 2.4|ansible 2.5|
|------------|-----------|-----------|-----------|
|alpine-latest|no|yes|yes|
|alpine-edge|no|yes|yes|
|archlinux|no|yes|yes|
|centos-6|no|no|no|
|centos-latest|np|yes|yes|
|debian-stable|no|yes|yes|
|debian-latest|no|yes|yes|
|debian-wheezy|no|no|no|
|fedora-latest|no|yes|yes|
|fedora-rawhide|no|yes|yes|
|opensuse-leap|no|yes|yes|
|opensuse-tumbleweed|no|yes|yes|
|ubuntu-artful|no|yes|yes|
|ubuntu-latest|no|yes|yes|

Example Playbook
----------------

```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.buildtools
    - role: robertdebock.epel
    - role: robertdebock.scl
    - role: robertdebock.python-pip
    - role: ansible-role-httpd
      httpd_applications:
        - name: myapplication
          location: /myapplication
          backend_url: http://localhost:8080/myapplication
        - name: myotherapp
          location: myotherapp
          backend_url: http://localhost:8080/myotherapp

  tasks:
    - name: place content
      copy:
        src: files/index.html
        dest: /var/www/html/index.html
```

Install this role using `galaxy install robertdebock.httpd`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
