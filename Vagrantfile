OS_IMAGE = "ubuntu/xenial64"
N = 2
Vagrant.configure("2") do |config|

  config.ssh.insert_key = false

  config.vm.define "master" do |master|
    master.vm.box = OS_IMAGE
    master.vm.network "private_network", ip: "192.168.50.10"
    master.vm.hostname = 'k8s-master'
    master.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--name", "k8s-master"]
      v.customize ["modifyvm", :id, "--cpus", 2]
    end
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "k8s-setup/k8s-master-setup.yaml"
      ansible.extra_vars = {
        node_ip: "192.168.50.10",
      }
    end
  end
  
  (1..N).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.box = OS_IMAGE
      node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
      node.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--memory", 1024]
        v.customize ["modifyvm", :id, "--name", "node-#{i}"]
        v.customize ["modifyvm", :id, "--cpus", 2]
      end
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "kubernetes-setup/k8s-node-setup.yaml"
        ansible.extra_vars = {
          node_ip: "192.168.50.#{i + 10}",
        }
      end
    end
  end
end
