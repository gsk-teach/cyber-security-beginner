# DeAuth Attack
> Without connecting to network, you can attach their client to force client to disconnect

Adv : Nothing much, just for fun

Logic:
1. Send a packet to router on the network imporsonating client (uses client's mac as source mac) with a message as "I want to disconnect"
2. Send a packet to client imporsontating router (uses routers mac as source) saying "Ok you are going to be disconnected"
3. Keep doing it a lot of times 

> Client can connect to other network (or say mobile data) and may not even notcie that he is disconnected from network

## How to you identify the network BSSID and Client? 
1. Peform targeted sniffing using airodump-ng which will show clients connected

## Usage
`aireply-ng --deauth 10000000000 -a BSSIDofNetwork -c StationOfClient [-D] wlan0`
> -D is only required for 5G network

