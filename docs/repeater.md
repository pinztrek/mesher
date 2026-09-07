# Recommended Repeater Settings

## Basic Settings
`set path.hash.mode 2`

`set flood.max 32`

`set flood.max.unscoped 16`

`set flood.max.advert 12`

`set loop.detect strict`

`set flood.advert.interval 47`

`set advert.interval 120`

`set agc.reset.interval 4`

## Setup Minimal regions (replace the us-ga-XX with your subregion)
`region def us-southeast us-ga us-ga-XX`<br>
`region save`

**See the [Georgia region commands document](https://github.com/pinztrek/mesher/blob/main/docs/ga_regions.md) for the exact commands for your geographic area**


## Keymind firmware variant settings (Strongly recommended for high sites)
**See the [Keymind Installation document in this repo](https://github.com/pinztrek/mesher/blob/main/docs/filtering_firmware_nodes.md) for how to install**

### Drop high hop #wardriving (loaded by default on keymind)
`set flood.rule.2 type=any channel=#wardriving hops=5+ drop`

### Drop any non-scoped wardriving that sneaks through
`set flood.channel.scope.require  #wardriving`

### Drop high hop Request/Anon flooding which are only relevant locally
`set flood.filter.3 req 5+`

`set flood.filter.4 anon_req 7+`

`set flood.filter.5 response 7+`

`set flood.filter.6 control 1+`

### Drop High Hop Bots
`set flood.rule.7 type=any channel=#test hops=9+ drop`

`set flood.rule.8 type=any channel=#bot hops=7+ drop`

`set flood.rule.9 type=any channel=#wx hops=7+ drop`

