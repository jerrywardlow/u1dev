# -*- mode: ruby -*-
# vi: set ft=ruby :

nodes = [
  {
    hostname:       "devweb",
    box:            "ubuntu/trusty64",
    config:         "web/web_config.sh",
    ip:             "101",
    synchost:       "web/",
    syncguest:      "/web"
  },
  {
    hostname:       "devdb",
    box:            "ubuntu/trusty64",
    config:         "db/db_config.sh",
    ip:             "102",
    synchost:       "db/",
    syncguest:      "/db"
  },
  {
    hostname:       "devfl",
    box:            "ubuntu/trusty64",
    config:         "fl/fl_config.sh",
    ip:             "103",
    synchost:       "fl/",
    syncguest:      "/fl"
  }
]

Vagrant.configure(2) do |config|
  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
  end
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.provision :shell, path: node[:config], args: node[:syncguest]
      nodeconfig.vm.box = node[:box]
      nodeconfig.vm.hostname = node[:hostname]
      nodeconfig.vm.network :private_network, ip: "172.66.166." + node[:ip]
      nodeconfig.vm.synced_folder ".", "/vagrant", disabled: true
      nodeconfig.vm.synced_folder node[:synchost], node[:syncguest]

      nodeconfig.vm.provider :virtualbox do |v|
        v.name = node[:hostname]
      end
    end
  end
end
