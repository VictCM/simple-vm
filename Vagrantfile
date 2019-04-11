# generate SSH key in the folder if they don't exist already
system('echo "Installing vagrant plugins required"')
system('[ $(vagrant plugin list | grep -c vagrant-libvirt) = "0" ] && vagrant plugin install vagrant-libvirt || echo "Vagrant-libvirt plugin detected"')


Vagrant.configure("2") do |config|

  $physical_interface = "enp0s25"    # interface name of your computer
  $public_ip = "172.30.0.172"    	   # free IP from your IP range

  $common_provisioning = <<-SHELL
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

# fixing vagrant local configuration; adding vagrant user to docker so that we can control containers
adduser vagrant docker
echo 'LC_ALL="en_US.UTF-8"'  >>  /etc/default/locale
echo 'LC_CTYPE="en_US.UTF-8"'  >>  /etc/default/locale
SHELL

  config.vm.define "simple-vm" do |simple_vm|
    
    simple_vm.vm.box = "ubuntu/bionic64"
    simple_vm.vm.provider :libvirt do |domain|
      domain.memory = 8192 
      domain.cpus = 2
      domain.nested = true
      domain.autostart = true
    end

    simple_vm.vm.network "public_network", bridge: $physical_interface, :dev => $physical_interface, ip: $public_ip
    simple_vm.vm.network "public_network", bridge: $physical_interface, :dev => $physical_interface, auto_config: false

    # installing packages
    simple_vm.vm.provision "shell", inline: $common_provisioning

  end

end
