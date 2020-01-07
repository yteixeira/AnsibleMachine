# Install required plugins
required_plugins = ["vagrant-hostsupdater"]
required_plugins.each do |plugin|
    exec "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

  # def set_env vars
  #   command = <<~HEREDOC
  #       echo "Setting Environment Variables"
  #       source ~/.bashrc
  #   HEREDOC
  #
  #   vars.each do |key, value|
  #     command += <<~HEREDOC
  #       if [ -z "$#{key}" ]; then
  #           echo "export #{key}=#{value}" >> ~/.bashrc
  #       fi
  #     HEREDOC
  #   end
  #
  #   return command
  # end

Vagrant.configure("2") do |config|
  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/xenial64"
    db.vm.network "private_network", ip: "192.168.10.150"
    db.hostsupdater.aliases = ["database.local"]
    db.vm.synced_folder "synced_files", "/home/ubuntu/synced_files"
    db.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "/home/ubuntu/synced_files/db_playbook.yml"
  end
end

config.vm.define "app" do |app|
  app.vm.box = "ubuntu/xenial64"
  app.vm.network "private_network", ip: "192.168.10.100"
  app.hostsupdater.aliases = ["development.local"]
  app.vm.synced_folder "synced_files", "/home/ubuntu/synced_files"
  app.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "/home/ubuntu/synced_files/app_playbook.yml"
  end
end
end
