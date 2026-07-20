# mesher

Coordinating MeshCore repeater deployment and sharing field experience.

## Objective

This repo purpose is the coordination point for planning, deploying, and tuning MeshCore
repeaters, and for capturing what we learn along the way. Repeater placement,
config, and behavior all affect the mesh for everyone on it, so the goal is to
make deployment decisions visible and documented rather than tribal knowledge
held by whoever installed a given node.

## Airtime Contention

LoRa airtime is a shared, finite resource — every repeater that rebroadcasts a
packet consumes airtime that every other node in range can't use at the same
moment. As repeater density increases, so does the risk of collisions, delayed
delivery, and reduced effective throughput for everyone sharing the channel.

Refer to the [Congestion & Airtime Contention Guide](docs/congestion.md) for 
additional detail & mitigation methods.


## Region Scoping

*Region scoping* is a a method built into *Meshcore* to reduce the core issues 
resulting from increased usage and connectivity. 

It is used as a traffic managament method to allow traffic to flood across 
multiple areas, but also restrict unneeded traffic from flooding beyond its
area of interest. 

The implementation specifics can be confusing, there are overlapping terminology
and arcane configuration commands. 

The [Meshcore Region Scoping Guide](docs/regions.md) addresses many points of confusion
and provides real world examples.
