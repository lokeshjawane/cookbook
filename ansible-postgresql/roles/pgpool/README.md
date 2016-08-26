# README #

This README would normally document whatever steps are necessary to get your PgpoolII setup & Running

### What is this repository for? ###

* Ansible role to configure pgpoolII setup node.

### How do I get set up? ###

* This script setup the pgpoolII nodes. Currently works for ubuntu14.04 OS version.
* Configuration Parameters are set in pgpoolII ansible role "role defaul/main.yml" file. Before running set the required parameter in the main.yml file.
#### Specify the data in defaul/main.yml
```sh
#user to check streaming replication,watchdog & health check. The <postgres user > should be enable for remote login with "trust" on master slave servers from pgpool
sr_check_user: '<postgres user>'
wd_lifecheck_user: '<postgres user>'
health_check_user: '<postgres user>'
pgpool_db_hosts: ['master', 'slave ip']
#below user should be present on the master slave & enabled with remote login from pgpoolII. 
pool_user: '<DB login user>'
pool_user_password: '<DB login password>'
```
# There is one encrypted vault file in "pgpool/vars/spoilers.yml" 
which contains public & private key. Default vault password is "machine", you can replcae the keys as in given format. make sure you the public key are added to "authorized_keys" to master & slave postgres user home folder.

# Create "site.yml" file
``` sh
- name: configure pgpoolII
  hosts: pgpool
  roles:
    - pgpool
```
#To run playbook use below command
ansible-playbook testing.yml -i hosts -e "postgres_version=9.5 bind_interface=eth0" --vault-password-file ./<vault password file> --sudo