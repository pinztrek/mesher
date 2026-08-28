# Firmware node policies using Keymind/halo

Intro

## Keymind + Halo + Cascade

  - **Keymind** — a rewritten flood-forwarding/filtering engine (file format "FPF7"). Replaces the old
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

## Example Region Scope Filtering & Remapping commands

### Map one region scope to another prior to forwarding
`set flood.channel.scope us-se us-southeast`

### Example Require scope for particular channels

```set flood.channel.scope.require #wardriving```
```set flood.channel.scope.require #bot```
