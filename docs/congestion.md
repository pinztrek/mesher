# Airtime Contention & Congestion

LoRa airtime is a shared, finite resource — every repeater that rebroadcasts a
packet consumes airtime that every other node in range can't use at the same
moment. As repeater density increases, so does the risk of collisions, delayed
delivery, and reduced effective throughput for everyone sharing the channel.

It can very quickly reach a point where only 1/3 to 1/2 of packets are
successfully received. 

As airtime contention gets worse, it becomes harder for *"Listen before transmit"* to work, and packets the repeater is trying to forward cannot be sent, and will expire and dropped. 

Users are often unaware, all they see are the packets from adjacent states (or local) which were lucky enough to get through.

This is not a new issue, it's existed as long as computer networking and made worse by LORA's half duplex limitation which leads to collisions on the best of days. And is made 
worse when *hidden terminal* impact occurs. (Stations which cannot hear another station
trying to send to the same repeater)


## Region Scoping

The use of *Meshcore region scoping* is one of the most effective methods 
of dealing with congestion / airtime contention due to traffic from adjacent areas. 

See full details at: [Region Scoping FAQ & Examples](docs/regions.md).

## Border Repeaters

It is quite common for *Meshcore* repeaters to have coverage areas that overlap multiple
geographic or metro areas. Likewise, if they are high sites, it can funnel an adjacent 
state's traffic far into adjacent states or metro areas. 

*Region scoping* alone will not address this. 

**Under Construction, more to come**
