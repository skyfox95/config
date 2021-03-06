# ICS Configuration on Ubuntu 16.04

Configuration of the network adapters file


Edit file */etc/network/interfaces* and add the following lines:

```
auto enp0s3
iface enp0s3 inet static
  address 192.168.0.101
  netmask 255.255.255.0
#  gateway 192.168.0.101
#  network 192.168.0.0
```


Configuration of the internet connection shared (with NAT)

Visit the [source](https://help.ubuntu.com/community/Internet/ConnectionSharing#Ubuntu_Internet_Gateway_Method_.28iptables.29) or this [website](https://dev-notes.eu/2016/08/persistent-iptables-rules-in-ubuntu-16-04-xenial-xerus/).

```bash
sudo iptables -A FORWARD -o enp0s3 -i enp0s8 -s 192.168.0.0/24 -m conntrack --ctstate NEW -j ACCEPT
sudo iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -t nat -F POSTROUTING
sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
```

Creates a backup of the rules: 

```bash
sudo iptables-save | sudo tee /etc/iptables.rules.backup
````

Edit file */etc/rc.local* and add the following lines before the *"exit 0"* line:
```bash
ptables-restore < /etc/iptables.rules.backup
```

Active forwarding 
```bash
sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
```

Edit file */etc/sysctl.conf* and uncomment the following line
```
net.ipv4.ip_forward=1
```


# Client configuration

```bash
sudo ip route add default via 192.168.0.101
```
or edit file /etc/network/interfaces and modify the right network adapter (enp0s3)
```
gateway 192.168.0.101
```

Edit file */etc/resolvconf/resolv.conf.d/base* and add the following lines

```
# Google dns
nameserver 8.8.8.8
nameserver 8.8.4.4
```

# Ubuntu Configuration 18.04.1 LTS

[route](https://askubuntu.com/questions/1052789/correct-way-to-route-between-2-interfaces-with-netplan-in-ubuntu-18-04)


Adds repository universe ofr sshpass package
```
sudo apt-add-repository universe
```


[dns configuration](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04)

[...](https://www.supinfo.com/articles/single/3498-installer-configurer-serveur-dns-linux)






