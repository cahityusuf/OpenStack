NUM_WORKER_NODE = 5
IP_NW = "192.168.60."
NODE_IP_START = 2

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
  #config.disksize.size = "55GB"
  config.vm.box_check_update = false
  # Provision Worker Nodes
  (1..NUM_WORKER_NODE).each do |i|
    config.vm.define "openstack0#{i}" do |node|
        node.vm.provider "virtualbox" do |vb|
            vb.name = "openstack0#{i}"
            vb.memory = 12000
            vb.cpus = 4
        end
        node.vm.hostname = "openstack0#{i}"
        node.vm.network :private_network, ip: IP_NW + "#{NODE_IP_START + i}"
                node.vm.network "forwarded_port", guest: 22, host: "#{2720 + i}"

        node.vm.provision "setup-hosts", :type => "shell", :path => "ubuntu/vagrant/setup-hosts.sh" do |s|
          s.args = ["enp0s8"]
        end

        node.vm.provision "setup-dns", type: "shell", :path => "ubuntu/update-dns.sh"
    end
  end
end