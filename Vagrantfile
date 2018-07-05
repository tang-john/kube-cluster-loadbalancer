Vagrant.configure("2") do |config|
  #config.vm.box = "ubuntu/xenial64"

  config.vm.define "kubemaster" do |v|
    v.vm.box = "johntang/kubemaster-metallb"
    v.vm.box_version = "1.0.0"
    v.vm.hostname = "kubemaster"
    v.vm.network "private_network", ip: "172.16.0.40"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
  end

  config.vm.define "kubenode1" do |v|
    v.vm.box = "johntang/kubenode1-metallb"
    v.vm.box_version = "1.0.0"
    v.vm.hostname = "kubenode1"
    v.vm.network "private_network", ip: "172.16.0.41"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
    end
  end

  config.vm.define "kubenode2" do |v|
    v.vm.box = "johntang/kubenode2-metallb"
    v.vm.box_version = "1.0.0"
    v.vm.hostname = "kubenode2"
    v.vm.network "private_network", ip: "172.16.0.42"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
    end
  end
end
