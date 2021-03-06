## FreeBSD Zabbix Agent

This small project is used for install zabbix_agent[5|6] on OS FreeBSD 13

## Dependencies

- Package on desktop - vagrant - 2.2.6+dfsg-2ubuntu3 Tool for building and distributing virtualized development environments
- Package on desktop - ansible - 2.9.6+dfsg-1 - Configuration management, deployment, and task execution system

## How it works

On Linux desktop run vagrant with VirtualBox. For test install and configure zabbix_agent by ansible on FreeBSD 13

### Installation test evnviroment FreeBSD

```console
vagrant up
vagrant ssh
```
- Vagrant also configure sshd - PermitRootLogin yes and set up root password and install ansible package

### Installation Desktop

- install ssh public key to FreeBSD

```console
cd ~/.ssh && ssh-copy-id -i id_rsa.pub root@192.168.42.100
cd ${HOME}/work/freebsd-zabbix-agent
```
- test Ansible communication
```console
ansible "*" -i "192.168.42.100," -u root -m ping
192.168.42.100 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/local/bin/python3.7"
    },
    "changed": false,
    "ping": "pong"
}
```
### Configure ansible on Desktop

```console
sudo vim /etc/ansible/hosts

[fbsd-vagrant]
freebsd ansible_ssh_host=192.168.42.100 ansible_ssh_user=root

ansible fbsd-vagrant -m ping
```
Use Ansible community collection general and pkgng module

- https://docs.ansible.com/ansible/latest/collections/community/general/index.html#plugins-in-community-general
- https://docs.ansible.com/ansible/latest/collections/community/general/pkgng_module.html

## Install Ansible module

```console
ansible-galaxy collection install community.general
or
ansible-galaxy collection install -r requirements.yml
```

## Run Ansible playbook with enviroment

```console
ansible-playbook zabbix-agent.yml --extra-vars "ansible_fqdn=freebsd zabbix_server=zabbix.pfsense.cz"
```

### ToDo

- Install and configure zabbix_server
