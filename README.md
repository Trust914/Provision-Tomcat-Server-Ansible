## Project Info

> This project contains two Ansible Roles to Install any version of Tomcat on CentOS, Fedora, Debian and Ubuntu Linux, clone a Java app repo and build the code


## Tested on the following operating systems

- CentOS 7
- Ubuntu 18.04

But should work on other Linux distros or with minimal tweeking

## Roles in the project

This project contains roles to:

- Install Tomcat on the servers
- Install maven, clone a repo and build the code

## Tasks in the tomcat role

This role contains tasks to:

- Install basic packages required
- Install Java
- Add tomcat user and group
- Download tomcat and install - configure systemd
- Configure firewall

## Tasks in the deploy-artifact role

This role contains tasks to:

- Install maven
- Clone a repo (should be Java based)
- Build the code


## How to use this role

- Clone the Project:

```
$ git clone https://github.com/Trust914/tomcat-ansible.git
$ cd tomcat-ansible
```

- Update your inventory, e.g:

```
$ vim inventory
[websrvgrp]
web01 ansible_host=172.168.10.10       # Remote user to act on
web02 ansible_host=172.31.93.72
web03 ansible_host=172.31.80.116 ansible_user=ubuntu # Add hostname, private ip and user if appropriate

[websrvgrp]
web01
web02
web03

[websrvgrp:vars]
ansible_user=centos
ansible_ssh_private_key_file=web-app-key.pem # Update your private key for ansible to do ssh

```

- Update variables in roles/deploy-artifact/defaults/main.yml file - Set git_url, git_branch, etc., as appropriate
- Update variables in roles/tomcat/defaults/main.yml file
- Update variables in provision-tomcat-server.yml file - Set Project name, java version, etc


If you are using non root remote user, then set username and enable sudo:

```
become: yes
become_method: sudo
```

## Running Playbook

Once all values are updated, you can then run the playbook against your nodes.

Playbook executed as root user - with ssh key:

```
$ ansible-playbook  provision-tomcat-server.yml 

```

Playbook executed as root user - with password:

```
$ ansible-playbook provision-tomcat-server.yml --ask-pass

```

Playbook executed as sudo user - with password:

```
$ ansible-playbook provision-tomcat-server.yml --ask-pass --ask-become-pass

```

Playbook executed as sudo user - with ssh key and sudo password:

```
$ ansible-playbook provision-tomcat-server.yml --ask-become-pass

```
Playbook executed as sudo user - with ssh key and passwordless sudo:

```
$ ansible-playbook provision-tomcat-server.yml --ask-become-pass

```

Execution should be successful without errors:

