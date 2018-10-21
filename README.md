# config


Configuration of the network adapters file:
Edit file '/etc/network/interfaces' and add the following lines:
```
auto enp0s3
iface enp0s3 inet static
  address 192.168.0.102
  netmask 255.255.255.0
  gateway 192.168.0.101
#  network 192.168.0.0
```
