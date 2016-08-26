# README #

This README would normally document whatever steps are necessary to get your Postgres Master slave setup & Running

### What is this repository for? ###

* Ansible Playbook to configure Postgres master slave setup with single master & slave node.

### How do I get set up? ###

* This script setup the master & slave server with single nodes. Currently works for ubuntu14.04 OS version.
* Replication Configuration Parameters are set in ansible template to create configuration file.
```sh
wal_level: hot_standby
archive_mode: on
archive_command: 'cd .'
max_wal_senders: 5
wal_keep_segments: 32
hot_standby: on
```

### Configuration & Setup ###
* Dependencies: All Dependencies are covered in playbook itself.
* Setup configuration & Steps to run playbook
```sh
#Add master slave server ip's or hostname to playbook hosts file
vim hosts
[master]
<master ip> ansible_connection=ssh ansible_ssh_user=<ssh login user>  ansible_ssh_private_key_file=<ssh pri key path>

[slave]
<slave IP> ansible_connection=ssh ansible_ssh_user=<ssh login user>  ansible_ssh_private_key_file=<ssh pri key path>
```
* Create encrypted vars file using below steps
```sh
#create vault password file
vim .vault_pass.txt
thepassword
:wq!
#Create vars.yml file
vim roles/master/vars/vars.yml
---
repl_password: <replication user password>
:wq

#envrypt vars file
ansible-vault encrypt roles/master/vars/vars.yml --vault-password-file ./.vault_pass.txt

Note: there is one more file to vault encrypted file : "roles/common/vars/spoilers.yml" default password is "machine". so decrypt it and reencrypt it with your own password.
```
* How to run playbook 
```sh
ansible-playbook site.yml -i hosts  -e "postgres_version=9.5 bind_interface=<ethernet name>" --vault-password-file ./.vault_pass.txt --sudo
Note: site.yml is master playbook file
```
* Your master slave are up & running now.