# Zerotier & OpenWrt for Zebra printers (Layer 2 network)

Set remote access to printers(zpl) from different servers using Layer 2 network, without ISP Router modification, thanks to Zerotier and OpenWrt.

There is a factory with several printers and two servers on diffrent hosts.

## IPs 

**ISP Router** (factory)
* WAN Dynamic IP (dont need to know)
* LAN IP 192.168.1.1 network 192.168.0/24
* LAN DHCP Server 192.168.1.30 - 192.168.1.199
* ZT no zerotier

**OpenWrt Router** (factory)
* LAN 192.168.1.200
* ZT zerotier-cli NoIP

**PRINTERS** (factory)
* LAN Assign 5 printers static IPs from 192.168.1.20 to 1920.168.1.25
* ZT no zerotier

**SERVERS** (dedicated servers on two different host)
* WAN two static IP (dont need to know) 
* ZT zerotier-cli 192.168.1.222 & 192.168.1.233

Looking for a router with Openwrt https://openwrt.org/ for this project, we found a Xiaomi Mi Router 4c, you can use faster router, but for zpl labels 100M speed is not a problem.

![xiaomi mi router 4c](/assets/images/xiaomi.png)



## Zerotier Configuration
* Create an account if you dont have one
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
Search your hardware and follow install instructions, important, you need to install an openwrt firmware **version** that allows to install zerotier, for example version used on this proyect 21.02.1

* Connect router to internet(use WAN connector) Install zerotier on the OpwnWrt Router, ssh to the router and run
```
opkg update
```
```
opkg install zerotier
```
* Add the router to zerotier network
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


* Connect to OpenWrt ip(192.168.1.1) open your brouser, configure LAN, ip static 192.168.1.200 network mask 255.255.255.0 disable DHCP, and ***restart router***
![new router ipaddress](/assets/images/openwrt0.png)
![new router ipaddress](/assets/images/openwrt01.png)

* Connect to OpenWrt(192.168.1.200) with web browser, create new network with device zt-----, set protocol unmanaged, (no IP).
![new ZTO](/assets/images/openwrt1.png)

* Goto devices, select br-lan, open devices selectors and check zt-----.
![new router ipaddress](/assets/images/openwrt00.png)
![add device to bridge](/assets/images/openwrt2.png)

* In the factory, connect WAN OpenWrt router port to the ISP Router LAN network, turn on OpenWrt router, and wait for lights stop blinking . Connect one OpenWrt router LAN port to the ISP Router LAN network, and you are done. (you can do with only one cable but you have to use other network configuration, dont ask me how to do). You can use the other OpenWrt Router LAN ports for your local network as a switch, and OpenWrt wifi.  

* Now, you can check that everything is working with a ping from your servers to every printer (and pc) on your factory network or you can check connections, doing a ping from a pc in the factory to the servers.













