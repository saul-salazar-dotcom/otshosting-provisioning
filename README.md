[DOCUMENTATION on wiki](https://github.com/DevelopersPL/otshosting-provisioning/wiki)

otshosting-provisioning
=======================
This is an Ansible playbook used to fully provision a Ubuntu machine for OTS Hosting.


## NOTE: This is a fork

This fork only changes the task `php-fpm` making it version agnostic, why? because the php version was hardcoded and causing troubles, for example with Ubuntu 22.04 LTS it will install PHP 8.1 and throw the next error:

```
ERROR! The requested handler 'restart php8.1-fpm' was not found in either the main handlers list nor in the listening handlers list
```


## Quick Start

__Supported OS: Ubuntu 20.04, 22.04__

Make sure to have universe, multiverse and restricted repositories enabled.

A script to run on a standalone machine to provision it. If user "otsmanager" does not exist, it will be created with password: "otsmanager".
```bash
#!/bin/bash -ex
apt-get update
apt install -y -q python-simplejson git-core ansible
ansible-pull -i localhost, -U https://github.com/DevelopersPL/otshosting-provisioning.git -d /srv/otshosting-provisioning --purge -t default

# Execute local files
git clone https://
cd otshosting-provisioning
ansible-playbook -c local -i localhost, -t default local.yml
```


## Uninstall

If you need to uninstall/unprovision this playbook, please read the [saulmendoza/otshosting-uninstall repo](https://gitlab.com/saulmendoza/otshosting-uninstall)


## Available tags

* systemd - enables persistent journald logging (default)
* general - software & integration (default)
* mysql - MariaDB SQL server (default)
* php-fpm - PHP support for website (default)
* nginx - web server (default)
* pma - phpMyAdmin for easy administration (default)
* tfs - TFS 1.X automatically compiled and installed (default)
* tfs-old - packages ONLY to compile older versions
* znote - ZnoteAAC automatically installed & configured (default)
* myaac - only installation, without configuration
* wine - wine packages to run exe (engines compiled for Windows)


## cloud-init based provisioning

A cloud-init script to provision a cloud instance using this playbook:
```
#cloud-config
users:
  - name: otsmanager
    gecos: OTS Manager
    lock-passwd: false
    
disable_root: true
ssh_pwauth: True
timezone: Europe/Warsaw

package_upgrade: true
package_update: true

packages:
 - python-simplejson
 - git
 - ansible
 - aptitude
 
runcmd:
  - 'ansible-pull -i localhost, -U https://gitlab.com/saulmendoza/otshosting-provisioning.git -d /srv/otshosting-provisioning --purge -t default'
```
