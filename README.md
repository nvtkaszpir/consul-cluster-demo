# Consul server cluster demo


# WARNING
This code is not for production use, only for educational purposes.

# About

Three different operating systems.
Create Consul cluster using Ansible roles.

# Requirements:

- locally installed vagrant with libvirt provider
- locally installed python + python-virtualenv (or pyenv), required for ansible

# Startup

Notice, the code is not fully tested and also it is better to execute certain
steps in sequence.

Example run under Ubuntu 16.04 as host:

```bash

sudo apt-get install -y python-virtualenv
virtualenv .venv
source .venv/bin/activate
pip install -r provisioning/ansible/requirements.txt
vagrant up --no-provision
vagrant provision --provision-with=shell

ansible-galaxy install --force -r provisioning/ansible/.galaxy/ -r provisioning/ansible/requirements.yml

ansible-playbook -vvv -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory provisioning/ansible/init.yml

```
