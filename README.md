# Zerotier & OpenWrt for Zebra printers (Layer 2 network)

Set remote access to printers(zpl) from direferent servers using Layer 2 network, thans to zerotier VPN.

There is a factory with several printers

## IPs

**ISP Router** 
IP 192.168.1.1 network 192.168.0/24
DHCP Server 192.168.1.30 - 192.168.1.199

**OpenWrt Router**
192.168.1.200

**PRINTERS**
Assign 5 printers static IPs from 192.168.1.20 to 1920.168.1.25

**SERVERS**
Two servers 192.168.1.222 & 192.168.1.223

We need a routed with Openwrt https://openwrt.org/ for this project we will use Xiaomi Mi Router 4c, you can use faster router, but for zpl labels speed is not problem.

## Zerotier Configuration
* Create and account if you dont have one
* Create a new network
* Configure network 
  
