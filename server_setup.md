#This is a walkthrough on how to setup a server

####Before Installing the server

1. Retrive MAC address
   1. Use `ifconfig` to retrieve MAC address from ethernet port (make sure to select the correct ethernet port as the server may have several different port, typically eno1) . Double check with the physical port that plus in the ethernet cable
2. Contact hostmaster or whoever responsible for assigning IP address in the VLAN and request a new IP address, subnetmask, and gateway. Also submit a prefered hostname which would be used for remote access. These information will be used in setting up netplan config.
3. Reserve a rack space with server colocation service (data center) with power supply and network preferences. Submit the network configuration to data centerfor network switch configuration.

#### Install server at the data center

1. Make an appointment with the data center.

2. Check the power cord and network cable.

3. Install click-on rail on the server.

4. Install the server on the rack frame.

5. Plug in power cord and network cable.

6. Power on the server and enter the shell using given username and password.

7. `cd /etc/netplan` and looking for `xxx.yml` file.

8. `sudo vim xxx.yml` and configure the `yml` file. It should looks like 

   ```bash
   # This is the network config written by 'subiquity'
   network:
     renderer: networkd
     ethernets:
       eno1:
         dhcp4: true
         dhcp6: false
         gateway4: xxx.xxx.xxx.xxx
         nameservers:
           addresses: [8.8.8.8, zzz.zzz.zzz.zzz, 8.8.4.4]
     version: 2
   ```

   Here `xxx.xxx.xxx.xxx `is your assigned ip address and `zzz.zzz.zzz.zzz` is the address for name server. You may need to edit `addresses `field based on your subnet mask. Note that in this case we use dhcp4. Also the `ethernets `field should also match with your ethernet port name with which the MAC address has been registered by hostmaster. 

9. `sudo netplan apply` to apply the netplan configuration.
10. `sudo hostname new-server-name-here` to change the host name. 

#### Enable SSH

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install openssh-client
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

To stop/restart ssh

```bash
sudo systemctl stop ssh
sudo systemctl restart ssh
```

To check the status of ssh

```bash
sudo systemctl status ssh
```

#### Add user with home directory and bash

```bash
sudo useradd -m -s $(which bash) USERNAME
sudo passwd USERNAME
```

##Congradualations, now you have a ssh-enabled server to use!

