# -*- mode: ruby -*-
# vi: set ft=ruby :

V_CPU = 2 # in cores
V_MEM = 1024 # in megabytes per core
V_MEM_TOTAL = V_MEM * V_CPU
SYNC_TYPE = "rsync" # how to sync files in vagrant, for lxc rsync is suggested


VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  if Vagrant.has_plugin?("vagrant-proxyconf")
      config.proxy.enabled = false
      config.proxy.http     = "http://192.168.0.2:3128/"
      config.proxy.https    = "http://192.168.0.2:3128/"
      config.proxy.no_proxy = "localhost,127.0.0.1,.example.com"
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true

  # providers
  config.vm.provider "virtualbox" do |v|
    v.cpus = V_CPU
    v.memory = V_MEM_TOTAL
  end

  config.vm.provider :libvirt do |libvirt, override|
    # this is vagrant, vm is disposable, so set up supper agressive disk access
    libvirt.cpu_mode = 'host-passthrough'
    libvirt.cpus = V_CPU
    libvirt.memory = V_MEM_TOTAL
    libvirt.random_hostname = true
    libvirt.video_type = 'vmvga'
    libvirt.video_vram = '16384'
    libvirt.volume_cache = 'unsafe'

  end

  # virtual machines

  config.vm.define "centos-1" do |s|
    s.vm.box = "centos/7"
  end
  config.vm.define "debian-1" do |s|
    s.vm.box = "generic/debian9"
  end
  config.vm.define "ubuntu-1" do |s|
    s.vm.box = "generic/ubuntu1604"
  end

  config.vm.provision "shell", path: "provisioning/shell/common.sh"

  config.vm.provision "ansible" do |ansible|
    # # notice, galaxy is executed per each node.
    ansible.galaxy_role_file = 'provisioning/ansible/requirements.yml'
    ansible.galaxy_roles_path = 'provisioning/ansible/.galaxy'

    ansible.compatibility_mode = "2.0"
    ansible.playbook = "provisioning/ansible/dummy.yml"
    ansible.groups = {
      "consul_instances" => [ "centos-1", "debian-1", "ubuntu-1"],
    }
    ansible.host_vars = {
      "centos-1" => { "consul_node_role" => "bootstrap"},
      "debian-1" => { "consul_node_role" => "server"},
      "ubuntu-1" => { "consul_node_role" => "server"},
    }
    ansible.verbose = 'v'
  end

end
