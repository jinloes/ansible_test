Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"
  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/mitchellh/vagrant/issues/5005
  config.ssh.insert_key = false
  config.vm.boot_timeout = 600
  config.vm.provider :virtualbox do |vb|
    vb.memory = 2048
    vb.cpus = 4
    vb.customize ['modifyvm', :id, '--nictype1', 'Am79C973']
    vb.customize ['modifyvm', :id, '--nictype2', 'Am79C973']
  end

  app_servers = {
    'jenkins' => {
      :name => 'jenkins',
      :ip => '192.168.10.10',
      :playbook => 'playbook.yml'
    },
    'database' => {
      :name => 'database',
      :ip => '192.168.20.10',
      :playbook => 'database.yml'
    }
  }

  app_servers.each do |key, value|

    config.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.playbook = value[:playbook]
      ansible.extra_vars = {
        mongo_bind_ip: "0.0.0.0"
      }

      config.vm.define value[:name] do |app_config|
        app_config.vm.network "private_network", ip: value[:ip]
      end # config.vm.define

    end # config.vm.provision

  end # app_servers.each

end