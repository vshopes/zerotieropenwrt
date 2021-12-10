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
* Configure network 192.168.1.0/24

  ![Configure network](/assets/images/zero1.png)

* Configure Auto Assign (optional)
![Configure network](/assets/images/zero2.png)

* On Ubuntu and Centos servers install zerotier-cli,
```
curl -s https://install.zerotier.com | sudo bash
```
```
zerotier-cli join [your-zerotier-network-ID]
```
* Return to zerotier and assign ips to the servers, delete ip assigned by zerotier
![server 1](/assets/images/zero4.png)
![server 2](/assets/images/zero3.png)

## OpenWrt
* Install OpenWrt on your router, go to 
```
https://openwrt.org/docs/techref/hardware/list
```
Search your hardware ando follow install instructions, important, you need to install an openwrt firmware **version** that allows to install zerotier, for example 21.02.1

* Install zerotier on the OpwnWrt Router, ssh to the router and run
```
opkg update
```
```
opkg install zerotier
```
* Add the router to our zerotier network
```
rm /etc/config/zerotier
touch /etc/config/zerotier
uci set zerotier.openwrt_network=zerotier
uci add_list zerotier.openwrt_network.join='[your-zerotier-network-ID]'
uci set zerotier.openwrt_network.enabled='1'
uci commit zerotier
/etc/init.d/zerotier restart
/etc/init.d/firewall restart
```

* Goto zerotier and configure Router, delete ip allow ethernet bridging
![openwrt router](/assets/images/zero5.png)

* Connect to OpenWrt ip(192.168.1.1) open your brouser, configure LAN, ip static 192.168.1.200 network mask 255.255.255.0 disable DHCP, ***restart router***
![new router ipaddress](/assets/images/openwrt0.png)
![new router ipaddress](/assets/images/openwrt01.png)
* Create new network with device zt-----, set protocol unmanaged, (no IP).
![new ZTO](/assets/images/openwrt1.png)
* Goto devices, select br-lan, open devices selectors and check zt-----.
![new router ipaddress](/assets/images/openwrt00.png)
![add device to bridge](/assets/images/openwrt2.png)
* In your factory connect WAN ethernet to your router, wait for lights, and then connect one lan to the router, (you can do with only one cable but you have to use other openwrt configuration) 
* Now you can ping from your servers to every printer (and pc) on your factory network and from every pc to the servers










