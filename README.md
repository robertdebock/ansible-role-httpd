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

- httpd_applications: a list of applications that will (reversed) proxy to the "backend_url". (See example for more details.)

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
|distro=alpine-3.6|yes|yes|yes|
|distro=alpine-3.7|yes|yes|yes|
|distro=archlinux|yes|yes|yes|
|distro=centos-7|yes|yes|yes|
|distro=debian-buster|yes|yes|yes|
|distro=debian-jessie|yes|yes|yes|
|distro=debian-stretch|yes|yes|yes|
|distro=debian-wheezy|yes|yes|yes|
|distro=fedora-26|yes|yes|yes|
|distro=fedora-27|yes|yes|yes|
|distro=opensuse-42.2|yes|yes|yes|
|distro=opensuse-42.3|yes|yes|yes|
|distro=ubuntu-artful|yes|yes|yes|
|distro=ubuntu-trusty|yes|yes|yes|
|distro=ubuntu-xenial|yes|yes|yes|

Example Playbook
----------------

```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.httpd
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
