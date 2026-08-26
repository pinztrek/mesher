# Regions are not Routes

In discussions we constantly see comments about *"using regions to extend traffic"* or similar. Usually as justification for bridging adjacent sub-regions by carrying both. 

*Which is exactly backwards from what regions perform!* 

*Regions* were implemented as a **traffic containment** tool. Specifically, to prevent traffic from spreading from it's relevance point. 

## Some local traffic becomes a nuisance outside of it's local area

Much traffic that is useful/needed locally is not useful outside of the local area. And is often a volume nuisance source. 
A prime example is **#wardriving** or **#test**. Both are valid tools and needed locallay. But outside of the local area they are
noise and contribute to airtime contention as they are repeated many hops. 

So repeater region configs do not **extend** coverage, they **contain** traffic. 

When a repeater carries 2 adjacent regions/subregions, it bypasses that containment, pushing the airtime/nuisance problem further out of
the targeted subregion. 

## But other traffic is of relevance outside of it's local area

Chat traffic is often topical, and of interest outside of its local area. And as such, needs to traverse regions. 

## Trans region traffic is controlled by scoping the traffic 

So how does a **user** get traffic out of their region? They do it by scoping their transmission more broadly. This is ok as:
- Chat traffic is a tiny percentage of overall traffic
- And Chat traffic has relevance outside of narrow subregions

By setting the broader scope (*us-southeast*, *us-XX* state level, etc) that traffic is traverses the multiple regions. 

*So there is no need for the repeater op to compromise the containment value by bridging adjacent regions.* 
Instead, it's a user education issue, but also starts with a simple & coherent region structure. Ex: this Georgia example
- *us-southeast* (Multi-state. Could also be *us*). 
- *us-ga* the entire state (default for most traffic)
- *us-ga-ne* or *us-ga-atl* Used to prevent high volume or potentially nuisance traffic from flooding outside the local area.

Default companion scope would be *us-ga*. Appropriate for most chat traffic, etc. 

*#public* or *#hamradio* or similar could be scoped more broadly. Set scope for that *#channel* to *us-southeast* or *us*. Flood as far as it can. 

But *#wardriving*, *#bot*, *#test*, etc should be scoped as local as possible. Does not make sense to allow to flood even 
state level. Use a sub-state subregion like *us-ga-ne* to keep local. 

**Remember: Traffic valid/useful locally can be a nuisance outside of it's intended area!**

## Companions hear all regions

Since companions hear all regions they will quickly see that a given *#channel* is scoped a certain way. The companion apps
are starting to show whether a chat is scoped or not. 

Likewise, periodic *#info* broadcasts with basic region recommendations and links will educate users. We already see that working. 

## But I have users outside of subregion X who want to participate in #mylocalchannel

Participation in local #channels is the most common reason folks think they need to bridge subregions. Which then
breaks the containment function. 

That problem surfaces because folks assume local #channel should be set to local scope. Instead, *regions* and *channels* are entirely orthagonal in operation. 
Two entirely different purposes.

It's a bit counterintuitive, but local chat channels should be set to a higher scope so they **do** flood more broadly. They are 
of value outside of the local area, they are topical in nature. Having a default companion scope of statewide (*us-ga*, etc) as
a default is a good start but it would not be a problem to have it scoped even more widely as traffic volume is low, but the topic
may have relevance outside the local area. 

