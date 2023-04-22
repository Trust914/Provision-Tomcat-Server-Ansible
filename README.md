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

- Update variables in roles/deploy-artifact/defaults/main.yml file - Set git_url and git_branch as appropriate

```
repo_dir: /tmp/repo/{{ project_name }}
git_url: https://github.com/Trust914/java-login-app.git  	   # Update to your required repo url
git_branch: master 						   # Update to the required branch in the git_url
```


- Update variables in roles/tomcat/defaults/main.yml file - Set tomcat_ver,tomcat_v_num,ui_manager_user, etc.

```
ui_manager_user: manager                    # User who can access the UI manager section only
ui_manager_pass: Str0ngManagerP@ssw3rd      # UI manager user password
ui_admin_username: admin                    # User who can access bpth manager and admin UI sections
ui_admin_pass: Str0ngAdminP@ssw3rd          # UI admin password

tomcat_archive_url: https://archive.apache.org/dist/tomcat/tomcat-{{ tomcat_v_num }}/v{{ tomcat_ver }}/bin/apache-tomcat-{{ tomcat_ver }}.tar.gz
tomcat_archive_dest: /tmp/apache-tomcat-{{ tomcat_ver }}.tar.gz
```


- Update variables in provision-tomcat-server.yml file - Set Project name, java version, etc

```
- name: Provision tomcat server
  hosts: websrvgrp
  become: yes               # If to escalate privilege
  become_method: sudo       # Set become method
  remote_user: root         # Update username for remote server
  vars:
    project_name: java-webapp-T
    tomcat_ver: 8.5.9
    tomcat_v_num: 8 
  roles:
    - tomcat
    - deploy-artifact
```

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

