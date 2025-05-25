Vagrant.configure('2') do |config|

  config.vm.define "postgresql" do |postgresql|
    postgresql.vm.box = "ubuntu/jammy64"
    postgresql.vm.box_version = "20241002.0.0"
    postgresql.vm.hostname = "postgresql"
    postgresql.vm.network "private_network", ip: "192.168.56.3"
    postgresql.vm.network "forwarded_port", guest: 5432, host: 5432
    postgresql.vm.synced_folder ".", "/vagrant", disabled: true
    
    postgresql.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 4
      vb.name = "postgresql_ubuntu"
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.inventory_path = "environments/local/hosts"
    ansible.vault_password_file = ".vault_password.txt"
  end

end
  
