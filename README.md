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
# install system core packages on controlling host
sudo apt-get install -y python-virtualenv

# install dependencies on controlling host (pyenv/python pip packages)
curl https://pyenv.run | bash

pyenv install
pyenv virtualenv --python=python3 ccd
pyenv activate ccd

# optional - get ansible dependencies
# ansible-galaxy install --force -r provisioning/ansible/.galaxy/ -r provisioning/ansible/requirements.yml

# spawn vms
vagrant up --no-provision
vagrant provision --provision-with=shell

# get ansible dependencies and generate inventory
vagrant provision --provision-with=ansible

# provision
ansible-playbook -vvv -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory provisioning/ansible/init.yml


```
