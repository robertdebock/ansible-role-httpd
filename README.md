httpd
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-httpd.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-httpd)

Provides Apache httpd for your system.

Context
--------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/robertdebock/robertdebock.github.io/artifacts/httpd.png "Dependency")

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

- robertdebock.bootstrap

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.3|ansible 2.4|ansible 2.5|
|------------|-----------|-----------|-----------|
|alpine-3.6|no|yes|yes|
|alpine-3.7|no|yes|yes|
|archlinux|no|yes|yes|
|centos-6|no|no|no|
|centos-7|np|yes|yes|
|debian-buster|no|yes|yes|
|debian-jessie|no|no|no|
|debian-stretch|no|yes|yes|
|debian-wheezy|no|no|no|
|fedora-26|no|yes|yes|
|fedora-27|no|yes|yes|
|opensuse-42.2|no|yes|yes|
|opensuse-42.3|no|yes|yes|
|ubuntu-artful|no|yes|yes|
|ubuntu-trusty|no|yes|yes|
|ubuntu-xenial|no|yes|yes|

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

Robert de Bock <robert@meinit.nl>
