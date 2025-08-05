# mesher
NW Atl Meshtastic test info
# Objective
Determine real world meshtastic node performance in the NW Atlanta suburban area (West Cobb specifically) on a (hopefully) uncongested frequency.

This should help us understand and potentially improve mesh operation on the very busy LongFast default channel on slot 20 (906.875). What we see now is many nodes are heard, but only 5-10% of DM's reach the intended station. Even less traceroutes. We suspect channel contention is a core issue due to many distant nodes running high max hop counts, overly frequent broadcasts, etc. 
# Method
Operate several several handheld, mobile, and fixed nodes within a 5-10 mile radius using a common channel definition (Frequency, name, key, modem preset, etc) and critical parameters to establish a baseline. 

Then incorporate a single moderately high (HAAT) site and see how that impacts usability. 
# Test Plan
## Common Channel definition
### Frequency:
For the test we will use Slot 50 on 914.375 as it is halfway between ham 927 repeater inputs and outputs, and to our knowledge does not have any mesh traffic on it. 
### Primary Channel:
*mesher* will be used as the primary channel, with the default PSK of AQ==, and the LongFast modem preset. The frequency is automatically set to slot 50 when the channel name of *mesher* is specified. 
*nokey* will be used as a 2nd channel, with a blank (empty) psk. secondary and higher channels always use the modem preset of the primary. 

The following url should be used to configure the channel:
<https://meshtastic.org/e/#Cg8SAQEaBm1lc2hlcjoCCCAKCxoFbm9rZXk6AggQEgwIATgBQAdIAVAeaAE>
## Other critical parameters:
- *maxhops* will be set to 3 for rooftop or higher nodes, and 5 for all others
- *nodeinfo* will be set to 900 for rooftop or higher nodes, and 1200-3600 for others
- normal Telemetry and position broadcasts are allowed
- telemetry infrastructure broadcasts are not allowed

### Whenever possible we will confirm communication with known good nodes when in range.

## Test Intercom via Signal
The following signal group will be utilized to coordinate, share results and observations, etc:
<https://signal.group/#CjQKICHKvDzE1d_s0lRKnHfxbDkyFsVysvCD4aT_GDJ0pRUjEhApi9pOCyxAmOFAPNdHw9RT>

## Test Plan Documentation:
This github will be used to store/record details, configs, etc

## Test config files:
Config files which can be used with the Android client can be found above. They will not overwrite nodenames, etc. But do overwrite other config items including channels, etc. 

You can do an export of your current configuration and save that file before loading the test configuration.

We need to test whether these will work across devices. But until then, I've saved config files for the T114 and t1000e. 

## Nebra and other Pi based configs running meshtasticd
Pi based systems require additional setup. As always, do a backup prior to executing. 
`meshtastic --export-config  | grep -v "No Serial Mesh" > myconfig.yaml` 

You can edit & restore at a later date using:
`meshtastic --configure myconfig.yaml`

### The following commands will set a meshtaticd device for the correct channel definition:

I have found meshtasticd to be finicky about editing channel definitions, and the delete option did not work. 

So for the test I would recommend clearing the channel definition by brute force:
```
# Clear your channel definition (may have to edit the path)
sudo systemctl stop meshtasticd
sudo rm /var/lib/meshtasticd/.portduinio/default/prefs/channels.proto
sudo systemctl start meshtasticd
```

```
# Set the channels for the test plan
# mesher on slot 50 with psk of AQ==
meshtastic --ch-set name "mesher" --ch-set psk default --ch-index 0
# nokey with blank psk, which will show in meshtastic --info as AA==, the internal representation of an empty PSK
meshtastic --ch-set name "nokey" --ch-set psk none --ch-index 1
# Set role, nodeinfo and maxhop
meshtastic --set device.role 2 --set device.node_info_broadcast_secs 900 --set lora.hop_limit 3
# edit to set your fixed location unless you are using GPS
meshtastic --setlon -84.656852 --setlat 33.909044 --setalt 300 --set position.position_broadcast_secs 3600
```
The above channel commands should yield a config that looks like this when `meshtastic --info` (or `m_info` on systems with my shims)
```
Channels:
  Index 0: PRIMARY psk=default { "psk": "AQ==", "name": "mesher", "moduleSettings": { "positionPrecision": 16, "isClientMuted": false }, "channelNum": 0, "id": 0, "uplinkEnabled": false, "downlinkEnabled": false }
  Index 1: SECONDARY psk=unencrypted { "psk": "AA==", "name": "nokey", "moduleSettings": { "positionPrecision": 16, "isClientMuted": false }, "channelNum": 0, "id": 0, "uplinkEnabled": false, "downlinkEnabled": false }

Primary channel URL: https://meshtastic.org/e/#Cg8SAQEaBm1lc2hlcjoCCBASDAgBOAFABUgBUB5oAQ
Complete URL (includes all channels): https://meshtastic.org/e/#Cg8SAQEaBm1lc2hlcjoCCBAKDhIBABoFbm9rZXk6AggQEgwIATgBQAVIAVAeaAE
```

## Participants:
|Ed kc4web||
|Alan km4ba|t1k, 2x t114, nebra 1w hat|
|Mike k8mdm|t1k, nebra meshtoad|
|Doug kd4nc|planned t1k, nebra 2w hat|
|Eddy wd3d||
|Ralph n4neq||
|Phillip n4trt|t1k?|
|||
|||