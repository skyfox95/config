# ics configuration

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

Creates a backup of the rules: 

```
sudo iptables-save | sudo tee /etc/iptables.rules.backup
````

Edit file */etc/rc.local* and add the following lines before the exit 0 line:
```
ptables-restore < /etc/iptables.rules.backup
```

Active forwarding 
```
sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
```

Edit file */etc/sysctl.conf* and uncomment the following line
```
net.ipv4.ip_forward=1
```


# Client configuration

```
sudo ip route add default via 192.168.0.101
```

Edit file */etc/resolvconf/resolv.conf.d/base* and add the following lines

```
# Google dns
nameserver 8.8.8.8
nameserver 8.8.4.4
```














