# Simple vm with Vagrant

I use Vagrant to create a simple clean envronment for testing

To initiate it you need to install Vagrant and his plugin in case you want a VM with more disk size:

```sudo apt install vagrant && vagrant plugin install vagrant-disksize```

Important: You may need to change the network interface from the Vagrantfile or the IP. You can change any of them with this command and <your_IP> or <your_NIC>:

```sed -i s/"ens18f1"/"<your_NIC>"/g Vagrantfile```
```sed -i s/"192.168.50.68"/"<your_IP>"/g Vagrantfile```

After that you can launch new VM:

```vagrant up```

and access it with:

```vagrant ssh```
