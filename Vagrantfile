VAGRANTFILE_API_VERSION = "2"
VAGRANT_DISABLE_VBOXSYMLINKCREATE = "1"
file_to_disk1 = './disk-0-1.vdi'
file_to_disk2 = './disk-0-2.vdi'
file_to_disk3 = './disk-0-3.vdi'
file_to_disk4 = './disk-0-4.vdi'
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
# Use same SSH key for each machine
config.ssh.insert_key = false
config.vm.box_check_update = false

# Repo Server
config.vm.define "repo" do |repo|
  repo.vm.box = "rdbreak/rhel8repo"
#  repo.vm.hostname = "repo.ansi.example.com"
  repo.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
  repo.vm.provision :shell, :inline => "yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm -y; sudo yum install -y sshpass python3-pip python3-devel httpd sshpass vsftpd createrepo", run: "always"
  repo.vm.provision :shell, :inline => " python3 -m pip install -U pip ; python3 -m pip install pexpect; python3 -m pip install ansible", run: "always"
  repo.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
  repo.vm.network "private_network", ip: "192.168.55.199"

  repo.vm.provider "virtualbox" do |repo|
    repo.memory = "512"
  end
end

# servera
config.vm.define "servera" do |servera|
  servera.vm.box = "rdbreak/rhel8node"
#  servera.vm.hostname = "servera.ansi.example.com"
  servera.vm.network "private_network", ip: "192.168.55.201"
  servera.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
  servera.vm.provider "virtualbox" do |servera|
    servera.memory = "1024"

    unless File.exist?(file_to_disk1)
      servera.customize ['createhd', '--filename', file_to_disk1, '--variant', 'Fixed', '--size', 2 * 1024]
      servera.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
      servera.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk1]
      end
  end
  
    servera.vm.provision "shell", inline: <<-SHELL
    yes| sudo mkfs.xfs /dev/sdb
    SHELL
    servera.vm.synced_folder ".", "/vagrant"
 end

# serverb
config.vm.define "serverb" do |serverb|
  serverb.vm.box = "rdbreak/rhel8node"
#  serverb.vm.hostname = "serverb.ansi.example.com"
  serverb.vm.network "private_network", ip: "192.168.55.202"
  serverb.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
  serverb.vm.provider "virtualbox" do |serverb|
    serverb.memory = "1024"

    unless File.exist?(file_to_disk2)
      serverb.customize ['createhd', '--filename', file_to_disk2, '--variant', 'Fixed', '--size', 2 * 1024]
      serverb.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
      serverb.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk2]
      end
 end
 
    serverb.vm.provision "shell", inline: <<-SHELL
    yes| sudo mkfs.xfs /dev/sdb
    SHELL
    serverb.vm.synced_folder ".", "/vagrant"
end

# serverc
config.vm.define "serverc" do |serverc|
  serverc.vm.box = "rdbreak/rhel8node"
#  serverc.vm.hostname = "serverc.ansi.example.com"
  serverc.vm.network "private_network", ip: "192.168.55.203"
  serverc.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
  serverc.vm.provider "virtualbox" do |serverc|
    serverc.memory = "512"

   unless File.exist?(file_to_disk3)
      serverc.customize ['createhd', '--filename', file_to_disk3, '--variant', 'Fixed', '--size', 2 * 1024]
      serverc.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
      serverc.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk3]
      end
  end
  
    serverc.vm.provision "shell", inline: <<-SHELL
    yes| sudo mkfs.xfs /dev/sdb
    SHELL
    serverc.vm.synced_folder ".", "/vagrant"
end

# serverd
config.vm.define "serverd" do |serverd|
  serverd.vm.box = "rdbreak/rhel8node"
#  serverd.vm.hostname = "serverd.ansi.example.com"
  serverd.vm.network "private_network", ip: "192.168.55.204"
  serverd.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
  serverd.vm.provider "virtualbox" do |serverd|
    serverd.memory = "512"

    unless File.exist?(file_to_disk4)
      serverd.customize ['createhd', '--filename', file_to_disk4, '--variant', 'Fixed', '--size', 2 * 1024]
      serverd.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
      serverd.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk4]
      end
  end
  
    serverd.vm.provision "shell", inline: <<-SHELL
    yes| sudo mkfs.xfs /dev/sdb
    SHELL
    serverd.vm.synced_folder ".", "/vagrant"
end

# Control Node
config.vm.define "control" do |control|
  control.vm.box = "rdbreak/rhel8node"
#  control.vm.hostname = "control.ansi.example.com"
  control.vm.network "private_network", ip: "192.168.55.200"
  control.vm.provider :virtualbox do |control|
    control.customize ['modifyvm', :id,'--memory', '2048']
    end
  control.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
  control.vm.provision :ansible_local do |ansible|
  ansible.playbook = "/vagrant/playbooks/master.yml"
  ansible.install = false
  ansible.compatibility_mode = "2.0"
  ansible.inventory_path = "/vagrant/inventory"
  ansible.config_file = "/vagrant/ansible.cfg"
  ansible.limit = "all"
 end
end
end
