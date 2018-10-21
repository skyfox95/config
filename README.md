# Host configuration


Configuration of the network adapters file


Edit file */etc/network/interfaces* and add the following lines:

```
auto enp0s3
iface enp0s3 inet static
  address 192.168.0.102
  netmask 255.255.255.0
  gateway 192.168.0.101
#  network 192.168.0.0
```


Configuration of the internet connection shared (with NAT)

Visit the [source](https://help.ubuntu.com/community/Internet/ConnectionSharing#Ubuntu_Internet_Gateway_Method_.28iptables.29).

```
sudo iptables -A FORWARD -o enp0s3 -i enp0s8 -s 192.168.0.0/24 -m conntrack --ctstate NEW -j ACCEPT
sudo iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -t nat -F POSTROUTING
sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
```

sudo iptables-save | sudo tee /etc/iptables.rules.backup

