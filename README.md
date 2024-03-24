# About the course
A beginner course on cyber security

# Contents
1. Cyber Security Introduction
2. Lab Setup
3. Linux Basics
4. Network Hacking Preparations
5. Preconnection attacks
6. Gaining Access - WEP Cracking
7. Gaining Access - WPA / WPA2 Cracking
8. Network Hacking - Securing your network


# Lab Setup
1. Download VMPlayer and custom Kali Linux image from zSecurity Site
2. Perform `sudo apt update` to ensure package repos are updated
3. Install Terminator (if not available) - `sudo apt install terminaror`
> Visit `explainshell.com` to understand the linux commands. You can also search google for `mediacollege linux commands` for linux commands 

# Network Hacking Preparations
> You need wifi adapters with supports `monitor mode`, `pocket injection` and `AP mode`. Chipset supported are `Realtek RTL 8812AU` or `Atheros AR9271`

1. Physically remove the wifi adapter
2. Open VM settings -> USB Connector -> change to 3.1 
3. Start Kali Linux VM
4. `ifconfig` -> you will see two adapters. One for natnetwork and other for loopback
4. Plug in wifi adapter and choose to connect to VM (not host network)
5. `ifconfig` -> this time you should see 3 adapters

## Change the MAC address of wifi adapter
> MAC (Media Access Control) address is physical, unique and permanent address of device. Access point router uses the MAC address for packet routing. Each package contains source MAC address and destination MAC address

### Why change MAC address? 
1. To become anounimous.
2. To imporsonate someone.
3. Bypass any filters on MAC address.

> To change any value of wifi adapter, we need to bring it down first. 

### Change MAC address
1. `ifconfig` -> find out wifi adapter you want to change the MAC
2. `ifconfig wlan0 down` to bring it down
3. `ifconfig` -> to see if it is down (you should see only two adapters now)
4. `ifconfig wlan0 hw ether 00:11:22:33:44:55` -> Ensure MAC address should start with 00
5. `ifconfig wlan0 up` -> to enable the wifi adapter 

> If you notice that MAC address is reset to original (without restarting pc), then we need to configure NetworkManager to retain MAC address change

** MAC will reset after PC reboot **

### To make NetworkManager preserve MAC address change
1. `nano /etc/NetworkManager/NetworkManager.conf` 
2. Add following at last
```
[connection]
ethernet.cloned-mac-address=preserve
wifi.cloned-mac-address=preserve
```
3. `service NetworkManager restart` -> to restart network manager
4. Now reattempt to change the MAC (all steps above). This time, MAC changes should preserve until machine is rebooted

