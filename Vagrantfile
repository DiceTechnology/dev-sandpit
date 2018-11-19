
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.hostname = "dce-sandpit"


  config.vm.provider "virtualbox" do |v|
    v.name = "dce-sandpit"
  end

  config.vm.synced_folder '~/img', '/img', disabled: false, owner: ENV['LOGNAME'], group: "admin"

  #########################################################
  # NETWORK
  #########################################################
  config.vm.network "private_network", ip: "192.168.50.1"

  # RabbitMQ Ports
  #config.vm.network "forwarded_port", guest: 5672, host: 5672
  #config.vm.network "forwarded_port", guest: 15672, host: 15672
  # MySQL Ports
  config.vm.network "forwarded_port", guest: 3306, host: 33060

  ###########################################################
  # SSH
  ###########################################################

  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/mitchellh/vagrant/issues/5005
  config.ssh.insert_key = false

  config.ssh.forward_agent = true
  config.ssh.port = 2222

  config.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key"]

  #config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
  #config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "~/.ssh/id_rsa"
  #config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "provisioning/playbook.yml"
    ansible.extra_vars = {
      dice_user: ENV['LOGNAME'], # works on mac
      aws_key: ENV['AWS_ACCESS_KEY_ID'],
      aws_secret: ENV['AWS_SECRET_ACCESS_KEY'],
      ansible_vault_key: ENV['ANSIBLE_VAULT_KEY']
    }
  end
end
