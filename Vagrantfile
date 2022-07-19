VAGRANTFILE_API_VERSION = '2'

Vagrant.require_version '>= 1.9.0'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  if Vagrant.has_plugin?("vagrant-timezone")
    config.timezone.value = "Etc/GMT+3"
  end

  config.vm.box = 'ovirt4'
  config.vm.box_url = 'https://github.com/myoung34/vagrant-ovirt4/blob/master/example_box/dummy.box?raw=true'
  config.vm.hostname = "foo"
  config.ssh.private_key_path = "~/.ssh/id_rsa"
  config.ssh.forward_agent = true
  ssh_pub_key = File.readlines("~/.ssh/id_rsa.pub").first.strip
  config.vm.provision 'shell', inline: 'mkdir -p /root/.ssh'
  config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /root/.ssh/authorized_keys"
  config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys", privileged: false

  config.vm.network :private_network,
    #:ovirt__network_name => 'ovirtmgmt' #DHCP
    # Static configuration
    :ovirt__ip => '192.168.10.101',
    :ovirt__network_name => 'ovirtmgmt',
    :ovirt__gateway => '192.168.10.254',
    :ovirt__netmask => '255.255.255.0',
    :ovirt__dns_servers => '192.168.10.1',
    :ovirt__dns_search => 'test.ru'
    :ovirt__interface_name => 'enp1s0'

  config.vm.provider :ovirt4 do |ovirt|
    ovirt.url = '<<url>>'
    ovirt.username = "<<login>>"
    ovirt.password = "<<password>>"
    ovirt.insecure = true
    ovirt.debug = true
    ovirt.cluster = '<<Cluster>>'
    ovirt.template = 'CentOS_7.6_Tmpl_Vagrant'
    ovirt.console = 'spice'
    ovirt.memory_size = '8 GiB' #see https://github.com/dominikh/filesize for usage
    ovirt.memory_maximum = '8192 MB'
    ovirt.memory_guaranteed = '8192 MB' #see https://github.com/dominikh/filesize for usage
    ovirt.cpu_cores = 2
    ovirt.cpu_sockets = 2
    ovirt.cpu_threads = 1
    ovirt.optimized_for = "Server"
    ovirt.description = "<<Description>>"
    ovirt.comment = "OL8"
  end

#  config.vm.provision "ansible" do |ansible|
#    ansible.verbose = "v"
#    ansible.playbook = "site.yml"
#    ansible.compatibility_mode = "2.0"
#  end
end
