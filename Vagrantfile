Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  VMS = 1
  (0..VMS-1).each do |vm|
    config.vm.define "server#{vm}" do |g|
        g.vm.hostname = "server#{vm}"
        g.vm.network :private_network, type: "dhcp"
        g.vm.provider :virtualbox do |vb|
            vb.memory = 2048
            vb.cpus = 2
        end

        if vm == (VMS-1)
            g.vm.provision :ansible do |ansible|
                ansible.playbook = "site.yml"
                ansible.groups = {
                    "storage" => (0..VMS-1).map { |v| "server#{v}" }
                }
                ansible.limit='all'
            end
        end
    end
  end
end
