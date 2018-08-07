Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = "1024"
    v.cpus = 2
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "70"]
  end

  config.vm.define "baseRH7" do |baseRH7|
#   baseRH7.vm.box = "generic/rhel7"
    baseRH7.vm.box = "iamseth/rhel-7.3"
    #baseRH7.vm.box = "javier-lopez/rhel-7.4"
    #baseRH7.vm.box = "xianlin/rhel-7.4"
    baseRH7.vm.hostname = "gomongoRH7"
    baseRH7.vm.network "private_network", ip: "192.168.60.182"
    baseRH7.vm.provision "shell", :inline => "sudo echo '192.168.60.182 baseRH7.local baseRH7' >> /etc/hosts"
         
    baseRH7.vm.provision "ansible" do |ansible|
      ansible.playbook = "deploy_baseRH7.yml"
      ansible.inventory_path = "vagrant_hosts"
      #ansible.tags = ansible_tags
      #ansible.verbose = ansible_verbosity
      #ansible.extra_vars = ansible_extra_vars
      #ansible.limit = ansible_limit
    end
  end
end
