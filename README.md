# httpd

Install and configure httpd on your system.

|Travis|GitHub|Quality|Downloads|
|------|------|-------|---------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-httpd.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-httpd)|[![github](https://github.com/robertdebock/ansible-role-httpd/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-httpd/actions)|[![quality](https://img.shields.io/ansible/quality/21855)](https://galaxy.ansible.com/robertdebock/httpd)|[![downloads](https://img.shields.io/ansible/role/d/21855)](https://galaxy.ansible.com/robertdebock/httpd)|

## Example Playbook

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  vars:
    httpd_locations:
      - name: mylocation1
        location: /mylocation1
        backend_url: "http://localhost:8080/myapplication"
    httpd_vhosts:
      - name: docroot
        servername: www1.example.com
        documentroot: /var/www/html/www1.example.com
      - name: backend_http
        servername: www2.example.com
        backend_url: "http://www.example.com/"
      - name: remote
        servername: www3.example.com
        remote: "http://localhost:3128/"
      - name: backend_https
        servername: www4.example.com
        backend_url: "https://www.example.com/"
      - name: piratebay
        servername: piratebay.nl
        backend_url: "https://thepirate-bay.org/"
        proxy_preserve_host: Off
        proxy_requests: Off
        setenv:
          - name: force-proxy-request-1.0
            value: 1
          - name: proxy-nokeepalive
            value: 1
          - name: proxy-initial-not-pooled
          - name: proxy-sendchunks
            value: 1

  roles:
    - role: robertdebock.httpd
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.buildtools
    - role: robertdebock.python_pip
```

For verification `molecule/resources/verify.yml` run after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: check if ports are open
      wait_for:
        port: "{{ item }}"
        timeout: 2
      loop:
        - "80"
        - "443"

    - name: interact with webserver
      uri:
        url: "https://127.0.0.1/mylocation1/"
        status_code: 503
        validate_certs: no
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## Role Variables

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for httpd

# The servername to use.
httpd_servername: "{{ ansible_fqdn }}"

# The non-SSL port to use.
httpd_port: 80

# To configure https, set the hostname to listen to.
httpd_ssl_servername: "{{ ansible_fqdn }}"

# For SSL a TCP port is required.
httpd_ssl_port: 443

# Set ProxyPreserveHost
httpd_proxy_preserve_host: On
```

## Requirements

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.buildtools
- robertdebock.epel
- robertdebock.python_pip
- robertdebock.selinux

```

## Context

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/httpd.png "Dependency")

## Compatibility

This role has been tested on these [container images](https://hub.docker.com/):

|container|tags|
|---------|----|
|alpine|all|
|el|7, 8|
|debian|buster, bullseye|
|fedora|31, 32|
|opensuse|all|
|ubuntu|focal, bionic, xenial|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.

## Exceptions

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| Alpine | ImportError: Error loading shared library /tmp/pip-build-env-23ZqyN/lib/python2.7/site-packages/_cffi_backend.so: Operation not permitted |
| amazonlinux | Dependency (python_pip) not available. |


## Testing

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-httpd) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-httpd/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## License

Apache-2.0


## Author Information

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
