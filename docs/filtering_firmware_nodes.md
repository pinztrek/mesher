# Firmware node policies using Keymind/Halo

High site meshcore repeaters are inevitably hit with airtime contention issues as the mesh grows. 
*Region scoping* can help reduce impact somewhat, but is not really granular enough. 
Other tools like *OpenHop* have implemented policy based forwarding decisions to help manage 
which traffic should be forwarded vs silently dropped as not-relevent and thus noise contributing to 
airtime challenges. 

[Mike Carper](https://github.com/mikecarper) has forked meshcore and extended it's capability 
to allow various flood mgt policies & rewriting.

*The intent / scope of this document is to help a repeater op install and configure commonly 
used flood mth configs*. The example policy settings are learnings from openhop where they 
have helped significantly by reducing forwarding of packets that are not relevant to local users. 
For full documentation and info please refer to the detailed configuration pages mentioned below. 

## Key additions: Keymind + Halo + Cascade
The release firmware typically has three extensions included:

  - **Keymind** — a rewritten flood-forwarding/filtering engine. Replaces the old
  simple flood.filter drop-list with a rule table where each row can match on payload type, hop
  count, authenticated channel, source-path prefix, and the packet's original scope/region, then
  take an action: drop, rewrite/force a transport scope (to a real region or an arbitrary regionless
  hashtag sink), rate-limit, allow it into the retry system, and/or stop lower-priority rules. It
  also adds flood.moderation (per-channel, per-username drop/rate/hop-limit for group text) and a
  shared blacklist. All of it is CLI-editable live, no reflash needed — it's essentially "smart"
  flood policy/moderation.

  - **Halo** — the retry/reliability layer sitting under that. Tunable direct-retry and flood-retry
  behavior (retry.preset infra/rooftop/mobile), a "recent repeater" heard-table, bridge-bucket mode
  for linking two separate repeater clusters (e.g. a north group and south group) so floods cross
  between them, ignore/prefix lists to control what counts as a successful retry echo,
  outpath/altpath direct-route overrides, mesh-based clock sync from adverts, telemetry history, and
  battery alerts. Basically topology-aware reliability tuning for repeaters in imperfect RF
  conditions (car/rooftop/mobile nodes).

  - **Cascade** — not a feature, it's a build/settings profile. bash build.sh build-firmware <target>
  --profile cascade selects an opinionated set of default saved settings (vs. stock --profile
  default), e.g. WiFi powersave defaults to min and CAD defaults to on on Cascade builds vs. off on
  target-default builds. It's the branding used for this fork's release/provider catalogs aimed at
  real-world deployments (see mesh-america/ provider JSONs, tagged things like
  halo-keymind-cascade).

  - **Summary**: *Keymind* = what gets forwarded and how it's moderated; *Halo* = how reliably it gets
  retried/bridged across topology; *Cascade* = the curated default-settings profile/release line
  bundling both for practical rooftop/mesh-community use.

## Download and Install

- - Go to Mike Carper's github for his Keymind fork: https://github.com/mikecarper/MeshCore/tree/keymindCascade
- - Click the [*releases* link](https://github.com/mikecarper/MeshCore/releases)(https://github.com/mikecarper/MeshCore/releases) in the right sidebar
- - Download the *firmware picker* html file. [*1.17.14 firmware picker*](https://github.com/mikecarper/MeshCore/releases/download/v1.17.1.4-halo-keymind-cascade-dev-4d5ccbdd/FIRMWARE-PICKER-1.17.1.4.html)
- - Open the *firmware picker* html file
- Select your hardware type, role (typically repeater), and install file type (often uf2 for nrf, etc)
- Use your normal install process to load the firmware onto your device. You can use the (https://flasher.meshcore.io/) page or drag and drop as appropriate for your device
- Use the *repeater setup* or *console* mode on the (https://flasher.meshcore.io/) page or similar to setup the repeater and access the console.
Should behave just like a normal meshcore 1.17'ish repeater.

## Check basic flood settings

- Follow the instructions in the [*Flood Filtering* doc](https://github.com/mikecarper/MeshCore/blob/keymindCascade/docs/flood_filtering.md)
to check/set recommended flooding configurations. (See below, may need to tune for your area)

## Configure flooding policies / remapping via the console
- Like above, use the console tool of your choice to enter the commands. There is a sample repeater section at the end of this doc. 

## Example Region Scope Filtering & Remapping commands

### Map one region scope to another prior to forwarding
`set flood.rule type=any channel=* in=region:us-se region=us-southeast`

### Example Require scope for particular channels

`set flood.channel.scope.require #wardriving`

`set flood.channel.scope.require #bot`

`set flood.channel.scope.require #test`

### Drop high hop count if unscoped
`set flood.rule type=any hops=9+ in=none drop`

## Sample key settings for a repeater in us-ga-atl

The following commands would be a good starting  point for a repeater to help reduce 
non-relevant forwarding similar to the Openhop approach we are using. 

You'll need to update the regions for your area. The example is for the *us-ga-atl*
sub-region. 

- Allow generous flood max for scoped traffic as it will self limit
  `set flood.max 64`

- Less generous flood max for unscoped traffic independent of path width
  `set flood.max.unscoped 32`

- Generous flood max for adverts
  `set flood.max.advert 32`

- Don't forward unscoped nuisance groups

  `set flood.channel.scope.require #wardriving`<br>
  `set flood.channel.scope.require #bot`<br>
  `set flood.channel.scope.require #test`
  

- Drop high hop count if unscoped
  `set flood.rule type=any hops=9+ in=none drop`

- Remap *us-ga* #wardriving to *us-ga-atl* to limit spread

  `set flood.rule type=any channel=#wardriving in=scope:us-ga region=us-ga-atl`

- Remap incorrect *us-se* to *us-southeast* (all channels)

  `set flood.rule type=any channel=* in=region:us-se region=us-southeast`

  *The above remapping examples assumes the regions exist already*. 
  For the case of incorrect regions, they need to exist, but your rule will remap them if encountered.
