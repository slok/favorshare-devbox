# Vagrantfile

VAGRANTFILE_API_VERSION = "2"

box      = 'ubuntu/trusty64'
hostname = 'devbox.favorshare.org'
ip       = '192.168.100.50'
ram      = '2048'
cpu      = '2'
project_location = "../favorshare"


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Box conf
  config.vm.box = box
  config.vm.host_name = hostname

  # Network conf
  config.vm.network :private_network, ip: ip

  # Ports
  config.vm.network :forwarded_port, guest: 8000, host: 8000 # Django
  config.vm.network :forwarded_port, guest: 5432, host: 15432 # Postgres

  # Synced folders
  config.vm.synced_folder project_location, "/home/vagrant/projects/favorshare"

  # Provider conf
  config.vm.provider :virtualbox do |vb|
    vb.customize [
      "modifyvm", :id,
      "--memory", ram,
      "--cpus", cpu,
      "--ioapic", "on",
    ]
  end

  # Ansible all the things!
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/main.yml"
    ansible.inventory_path = "provisioning/inventory"
    ansible.verbose = "v"

    # Check this https://github.com/mitchellh/vagrant/issues/3096
    ansible.limit = 'local'
  end

end
