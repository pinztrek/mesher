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

It can very quickly reach a point where only 1/3 to 1/2 of packets are
successfully received. 

As airtime contention gets worse, it becomes harder for *"Listen before transmit"* to work, and packets the repeater is trying to forward cannot be sent, and will expire and dropped. 

Users are often unaware, all they see are the packets from adjacent states (or local) which were lucky enough to get through.

This is not a new issue, it's existed as long as computer networking and made worse by LORA's half duplex limitation which leads to collisions on the best of days. And is made 
worse when *hidden terminal* impact occurs. (Stations which cannot hear another station
trying to send to the same repeater)

## Region Scoping

*Region Scoping* was designed into *meshcore* as a learning from *Meshtastic* to deal
with airtime contention from unbounded flooding as usage increases. 

### Region Learnings & Observations

1. **Region Scope *only* determines if a packet is forwarded by a repeater or not**
If the region in a packet is defined in the repeater's region table, the packet is 
forwarded. If not, it's dropped. Forwarding of *unscoped packets* is determined by a 
separate setting. (See examples below)


2. **Region name is not a route**
   - The region name is simply that: a unique identifier
   - All regions get hashed to 2 byte numbers for usage by the repeater in forward decisions. This hashed code is called the *transport code*, is present in packets and is a simple yes/no check. If the packet has a transport code in the table, it is forwarded.
   - Even though some region names *(us-ga-atl)* imply a hierarchy, it's for readability / administrative purposes only. **The repeater only checks to see if a packet matches the *transport code* when deciding whether to forward**

3. **Region Scope is entirely different than #channels**- They do not have to align, and in fact in many cases you will want different *region scope* for different channels depending on purpose

4. **Companions receive all packets without regard to region scope**- Companion region settings (default or per channel) only impact what *region scope* is used for packets it sends.

5. **Region Scoping is completely different than regions for mapping**- For valuable tools like meshmapper, *region scoping* may be completely different than the defined *geographic regions* used to collect and display wardriving coverage plots, packet analysis tools, etc.

   This issue has led to much confusion. Mapping/analysis tools have recommended IATA airport codes. This makes sense for their purposes. And it make make sense for *region scoping* structure in some cases. But **it is not a requirement for region scoping to operate**.

There could be an advantage to aligning the mapping regions to the RF regions, but they serve different purposes. 

6. **Repeater "Default Region" only impacts packets it _generates_**- Advertisements being a primary example.

7. **Region Scoping is the primary method of dealing with airtime contention**- As deployments build out, traffic from adjacent states or metro areas start to compete with local. *Region scoping* allows users/repeaters to control whether traffic from adjacent regions should contend with local traffic.

   Examples:
   - **#public** it would be appropriate to use a state or even multi-state *region scope* to communicate. You *want* the traffic to traverse adjacent regions.
   - **#wardriving** As high volume traffic that only exists to communicate with local repeaters, it should be tightly scoped to your immediate area to prevent deluge. *This is a significant problem in the SE, with wardriving traffic from 3 states away creating airtime contention*.
   - **#test or #bot** Similar to wardriving, testing is inappropriate to use wider scoping. Metro or state level might be appropriate to get more than one bot response.

8. **Unscoped Packets**- Meshcore uses the * indicator to designate unscoped traffic. It is **not** a wildcard, it's just a shorthand for packets without a *transport code* scope.

   Typically there is a setting to allow unscoped packet forwarding in repeaters, or it may be set in your region configuration statement depending on firmware level.

9. **# often precedes regions, but is not typed/saved during configuration**- This confuses many. When entering region names, just type the name (Ex: *us-ga*)

10. **Companion Clients do not support regions well**- *Region Scoping* is a new capability. The companion clients are adding support, but they are behind. But companions don't need much, just the capability to designate a region scope to be used in a #channel, and also (optionally) for adverts, etc.

    Kieker is an exception, region support is much more visible and you can see if regions are in use on particular transmissions.

    Most clients allow you to add regions when you try to scope a #channel to a region. Then you can select or unselect as needed. 
*The complicated and arcane region structures are not needed for companions!*

11. **Defining region scope on firmware repeaters is cryptic**- Check your firmware for specific commands to configure regions (see example firmware commands below)

12. **There are multiple approaches to defining regions for an area**- The wonderful thing about standards is that there are so many to choose from. (See example structures below)

    This is very true with *region scoping*- Thera are some well defined approaches used by many contries, at least at the top level. But they can diverge at lower levels based on local need.

    There are also many counter examples, see below in the **Region Structure** section.

13. **Region structure needs consensus**- The region structure for your area should be a work of collaboration and should be as standard as possible while allowing/enabling functionality and managing airtime contention.

14. ***Standard*** **is better than** ***better***- It is more important for a state or metro 
area to agree at state level *and implement/use* a consistent approach than it is 
to have the optimal subregion definition which never is agreed to. 
Which leads to the following point.

15. **It's critical to get State level in usage**- State level *region scoping* is needed now by many areas. It should be easy to agree on, the strong precedent is something this form: **us-XX** (Where XX is the state abreviation).

    It is very easy to get hung up on defining subregions, when the biggest airtime contention is at state level.

16. **A sub-region should be able to define it's name and coverage area**- It's easy to try to define things at the state level for all subregions, etc. You'll run into resistance/adoption issues based on boundaries, names, etc.

    The subregions exist to serve that particular area.

17. **Radio waves do not follow map boundaries**- *Region scoping* structure should factor in metro areas, rough state geographic "sub-regoins" (NE, NW, SE, etc). The purpose is to deal with airtime contention, so thought should be given to the mesh coverage zones more than county boundaries.

18. **Metro areas can cross state boundaries**- In many cases a metro area will straddle a state boundary. One local example would be Chattanooga TN. Clearly would be part of *us-tn* region. But the reality is that it has many Alabama and Georgia suburban communities as part of it's metro area. Given that, **us-tn-cha** would be an approprite region as an example. **us** to indicate it's part of the US and make it unique. **cha** as the IATA 3 digit code for the local airport.

    Yet users in AL or GA could use it for traffic intended for the metro area.

19. **Meta regions are OK**- Many times it's convenient to have meta regions composing other regions. A prime example would be something like **us-atlantic, us-southeast or us-se** which would include several states. Should be used selectively, but has a valid usage.

    If there is not consensus, carrying duplicate names for the same area is probably better than stalling. Companions will hear it either way. Ex: *us-southeast* and *us-se*. If you don't have agreement, carry both. Other states will have input.

20. **simple/shorter is good**- Region configurations often have to be hand typed in a cryptic format on phones. Likewise, there is a byte limit on the length of the string returned by the **discover regions** meshcore operation.

## Example Region Structures
- **us** (OR)
- **us-southeast**
  - **us-ga**
    - **us-ga-nw**
    - **us-ga-ne**
    - **us-ga-sw**
    - **us-ga-se**
    - **us-ga-atl**

Again, the hierarchy is to help humans. The repeater only cares that the region
*transport code* matches to determine whether to forward or not. 

Most countries with advanced *Meshcore* deployments have used country code, then 
either state/provice abreviation, then 3 digit ISO metro code. (But not all)

Also, most stack them hierarchical with hyphens (recommended, ex: us-ga-atl)

This is the dominant approach in the US, with some using metro names and others 
picking well known airport IATA codes. 

But several US states just have their subregion names:
- **pnw**
  - **or**
  - **wa**
    - **sea**
    - **spokane**

It's a name, not a route. So as long as it's unique in likely coverage areas it 
probably does not matter

21. **Repeaters should *not* carry all the regions for a state**- That defeats the 
purpose. It should only carry the regions it *should* forward traffic for. 

For the example above, a repeater in the metro Atlanta area should have:

    - **us-southeast**
      - **us-ga**
        - **us-ga-atl**

    And perhaps some legacy regions like *#atlanta* or similar if needed.

North East GA might have:

    - **us-southeast**
      - **us-ne**

    (Plus any local subregions for cities, etc they might want)

22. **Including the us region is ok, but not needed** (Opinion) Given the core purpose
is to mitigate airtime constraints, a country wide region covering an area like the US 
is probably unneccessary and unneeded. More imporantly, it could make airtime issues 
worse if misused. 

Note that including the us as the start of the region name is fine and even recommended.
ex: **us-va**


23. **Region Scoping does not solve all problems**- Radio waves travel line of site. A high repeater near a state boundry can create issues as:

    - The tendancy is to allow traffic from both states
    - Are also typically heard far into the interior of both states

    This effectively funnels two or more states of traffic far into the interior of a state.

    There are RF methods to mitigate, but just recognize that as traffic increases the 
scoping applied to high sites may need to change and become more restrictive.

## Typical firmware commands to set regions

Depending on firmware level, configuring for the representative areas above would be:

**For Metro Atlanta area (roughly defined as 30m radius of the capital):**
**16.x**
```
region def us-southeast us-ga us-ga-atl|* atlanta
region save
region allowf *q
```

- the allowf enables unscoped flooding
- the order of the regions implies hierarchy, with | starting a new top level

**15.x**
```
region put us-southeast
region put us-ga us-southeast
region put us-ga-atl us-ga
region put atlanta
region save
```

- 15.x allows unscoped flooding by default
- Parent / child relationship is specified in each line (the 2nd region listed is parent)

**For NorthEast Georgia region it would be:**
**16.x**
```
region def us-southeast us-ga us-ga-ne
region save
region allowf *q
```

**15.x**
```
region put us-southeast
region put us-ga us-southeast
region put us-ga-ne us-ga
region save
```

**Note:** the above configurations are recommendations in use for NE GA and the Metro 
Atl area. Other GA regions would likely follow the format/approach. 

And other meta regions or local subregions can be added as needed. 
