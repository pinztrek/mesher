# Recommended Repeater Settings

## Basic Settings
`set path.hash.mode 1`

`set flood.max 32`

`set flood.max.unscoped 16`

`set flood.max.advert 12`

`set loop.detect strict`

`set flood.advert.interval 19`

`set advert.interval 120`

`set agc.reset.interval 4`

## Setup Minimal regions (replace the us-ga-XX with your subregion)
`region def us-southeast us-ga us-ga-XX`

`region save`


## keymind settings (Strong recommended for high sites)
**See the [Keymind Installation document in this repo](https://github.com/pinztrek/mesher/blob/main/docs/filtering_firmware_nodes.md) for how to install**

### Drop high hop #wardriving (default load on keymind)
`set flood.rule.2 type=any channel=#wardriving hops=5+ drop`

### Drop any non-scoped wardriving that sneaks through
`set flood.channel.scope.require  #wardriving`

### Drop high hop Request/Anon flooding, only relevant locally
`set flood.filter.3 req 5+`

`set flood.filter.4 anon_req 7+`

`set flood.filter.5 response 7+`

`set flood.filter.6 control 1+`

### Drop High Hop Bots
`set flood.rule.7 type=any channel=#test hops=9+ drop`

`set flood.rule.8 type=any channel=#bot hops=7+ drop`

`set flood.rule.9 type=any channel=#wx hops=7+ drop`

