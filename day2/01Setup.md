# Lab Setup
1. Download VMPlayer and custom Kali Linux image from zSecurity Site
2. `sudo apt update` -> to ensure package repos are updated
3. `sudo apt install terminaror` -> Install Terminator (if not available)
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

__MAC will reset after PC reboot__

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

## Change to Monitor Mode
- Network had packets and each packet had source and destination MAC address. 
- Each device receieves only those packets which have it's destination address
- If we are within the range, we can capture all packets irresoective of destination address if wifi adapter is in monitor mode (by default, wifi adapter is in managed mode)
- We can change mode to monitor if it is supported by wifi adapter

1. `iwconfig` -> lists wireless adapters only. Observe the mode.
2. `ifconfig wlan0 down` -> to bring down the wifi adapter
3. `airmon-ng check kill` -> to kill any process including network manager. Internet may go down. We don't need internet to capture the pockets
4. `iwconfig wlan0 mode monitor` -> change to monitor mode
5. `iwconfig` -> to check the mode
6. `ifconfig wlan0 up` 


