# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  $physical_interface = "ens18f1"  # interface name of your computer
  $public_ip = "192.168.50.68" 	   # free IP from your IP range

  config.vm.box = "ubuntu/xenial64"
  config.disksize.size = '120GB' # requires a vagrant plugin
  
  config.vm.network "public_network", bridge: $physical_interface, ip: $public_ip # API and web access
  config.vm.network "public_network", bridge: $physical_interface, auto_config: false # Network interface for VMs

  config.vm.provider "libvirt" do |v|
      v.name = "simple-vm"
      v.memory = 16192 
      v.cpus = 4
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"] 
      v.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
  end

  config.vm.provision "shell", inline: <<-SHELL

# just installing some packages I usually need
apt-get update
apt-get -qy install python-pip
apt-get -qy install python-dev libffi-dev gcc libssl-dev python-selinux python-setuptools
pip install ansible 

# Install packages to allow apt to use a repository over HTTPS
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# add docker repository and instll it
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 
sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs) stable"
sudo apt-get update

sudo apt-get -y install docker-ce
sudo curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
# executing docker without sudo
sudo usermod -aG docker ${USER}
su - ${USER}


SHELL



# fixing vagrant local configuration; adding vagrant user to docker so that we can control containers
config.vm.provision "shell", inline: <<-SHELL
adduser vagrant docker
echo 'LC_ALL="en_US.UTF-8"'  >>  /etc/default/locale
echo 'LC_CTYPE="en_US.UTF-8"'  >>  /etc/default/locale
SHELL

end
