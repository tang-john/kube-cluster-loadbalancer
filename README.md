# kube-cluster-loadbalancer

Use the Vagrantfile in this repository to spin up a Kuberntes three node cluster.  This repository takes [kube-cluster](https://github.com/tang-john/kube-cluster) and adds  [MetalLB](https://metallb.universe.tf/) to enable the LoadBalancer service type.


## Requirements
The following software must be installed prior to executing these instructions.

* Vagrant
* Virtual Box
* Windows 10 or Ubuntu
* IP Addresses
  - 172.16.0.40 for kubemaster
  - 172.16.0.41 for kubenode1
  - 172.16.0.41 for kubenode2
  - 172.16.0.130 to 172,16.0.150 for deployments that require LoadBalancer service type.

Make sure these IP addresses are not being used by any other computer or VMs. If you need to use different IP addresses execute Windows Instructions or Ubuntu Instructions below first then see [Custom IP](https://github.com/tang-john/kube-cluster/blob/master/CUSTOM-IP.md).

## Windows Instructions
These instructions will use d:\data\vm\vagrant\kubernetes\01-cluster\logs directory for logs but you can use any directory.

 * Create a directory with path d:\data\vm\vagrant\kubernetes\01-cluster\logs
 * Copy file Vagrantfile to d:\data\vm\vagrant\kubernetes\01-cluster\
 * Start a command shell and go to d:\data\vm\vagrant\kubernetes\01-cluster\
 * Execute from the command shell the following commands to download Vagrant boxes
    - vagrant box add johntang/kubemaster-metallb --box-version 1.0.0 --provider virtualbox
    - vagrant box add johntang/kubenode1-metallb --box-version 1.0.0 --provider virtualbox
    - vagrant box add johntang/kubenode2-metallb --box-version 1.0.0 --provider virtualbox

 * Execute: vagrant up
    - An error, VERR_PATH_NOT_FOUND, will occur when the kubemaster VM starts. Start the VirtualBox Admin GUI from the Windows start button and execute the following instructions.
      - Look at the VM at the bottom with kubemaster in the name. Click on the name.
      - Click Settings from the menu bar.
      - Click on Serial Ports.
      - Change the Path/Address to d:\your-custom-folder\kubemaster.log such as D:\data\vm\kubernetes\01-cluster\logs\kubemaster.log
      - Click OK
      - Go back to the command shell and execute: vagrant up
      - Repeat step 5 instructions to fix kubenode1 and kubenode2 log path. Make sure to use kubenode[1|2] instead of kubemaster in the instructions.



## Ubuntu Instructions
These instructions will use /data/vm/vagrant/kubernetes/01-cluster/logs/ directory for logs. If you decide to use any other directory you will need to execute some extra steps.

 * Create a directory with path /data/vm/vagrant/kubernetes/01-cluster/logs/
 * Copy file Vagrantfile to /data/vm/vagrant/kubernetes/01-cluster/
 * Start a command shell and go to /data/vm/vagrant/kubernetes/01-cluster-metallb
 * Vagrant will download boxes defined in Vagrantfile if they are used for the first time. However, we will download the boxes manually for easier execution of step 5. Execute from the command shell the following lines.
    - vagrant box add johntang/kubemaster-metallb --box-version 1.0.0 --provider virtualbox
    - vagrant box add johntang/kubenode1-metallb --box-version 1.0.0 --provider virtualbox
    - vagrant box add johntang/kubenode2-metallb --box-version 1.0.0 --provider virtualbox
 * vagrant up
    - Follow these instructions if you use any other directory other than /data/vm/vagrant/kubernetes/01-cluster-metallb
      - An error, VERR_PATH_NOT_FOUND, will occur when the kubemaster VM starts. Start the VirtualBox Admin GUI from the Ubuntu start button.
      - Look at the VM at the bottom with kubemaster in the name. Click on the name.
      - Click Settings from the menu bar.
      - Click on Serial Ports.
      - Change the Path/Address to /your-custom-folder/kubemaster.log such as /data/vm/vagrant/kubernetes/01-cluster-metallb/kubemaster.log
      - Click OK
      - Go back to the command shell and execute: vagrant up
      - Repeat instructions to fix kubenode1 and kubenode2 log path. Make sure to use kubenode[1|2] instead of kubemaster in the instructions.


 ## Validation
 Follow these instructions to ensure that the cluster is working correctly. Make sure the VMs have started without any errors using the "vagrant up" command.

 * Make sure your command shell is in the same directory as the Vagrantfile.
 * Execute: vagrant ssh kubemaster 
 * Execute: kubectl get pods
 * Exeucte: kubectl get services
 * Look for the nginx service's External IP address. It should be 172.16.0.130.  There are two instances of the nginx pod and they are being load balanced by MetalLB. 
 * Open a web browser and navigate to 
