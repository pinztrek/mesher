# Georgia Meshcore Region Information

## Design Objectives
### Focus on traffic management
   - Core issue is to manage flooding leakage that is creating issues with adjacent
   zones and states

### Keep it as simple as possible

   - Intuitive zones, no map or rules needed
   - Minimal number of zones to allow interstate traffic and restrict to local
   - Minimal of commands to execute on repeaters to set up regions

### Align to defacto standard 
   - us-xx-yy* is the international standard and adopted by many US states

### Avoid future name duplication with other states
   - The us-xx-yy avoids potential name clashes with other countries, or subregions 
   in adjacent states. 

See: [Region Scoping FAQ](docs/regions.md) for *why* and how regions work.

## Structure

```
us-southeast
   us-ga
      us-ga-xx (quadrant zone) or us-ga-xxx (iata metro area)
```

## Timeframe

### Plan/Educate (Started)
- obtain consensus on *us-ga* + *us-southeast* (complete)
- document known subregions
- meshmaper region set to use *us-ga*
- established contact with key hi site ops
- get the word out to users

### Run in Parallel Oct 1
- key high sites have *us-ga*, *us-southeast*, and their subregion configured 
- limit unscoped flooding to 5-6 hops
- User education push

### Complete Cutover Jan 1
- forwarding of unscoped disabled

## Configuration Commands

| NorthEast GA | NorthWest GA (proposed)|
| :--- | :--- |
| *Version 1.16+*<br>`region def us-southeast us-ga us-ga-ne`<br>`region save`| *Version 1.16+*<br>`region def us-southeast us-ga us-ga-nw`<br>`region save` |
| *Version 1.15*<br>`region put us-southeast`<br>`region put us-ga us-southeast`<br>`region put us-ga-ne us-ga`<br>`region save`| *Version 1.15*<br>`region put us-southeast`<br>`region put us-ga us-southeast`<br>`region put us-ga-nw us-ga`<br>`region save` |

| SouthEast GA (proposed) | SouthWest GA (proposed) |
| :--- | :--- |
| *Version 1.16+*<br>`region def us-southeast us-ga us-ga-se`<br>`region save`| *Version 1.16+*<br>`region def us-southeast us-ga us-ga-sw`<br>`region save` |
| *Version 1.15*<br>`region put us-southeast`<br>`region put us-ga us-southeast`<br>`region put us-ga-se us-ga`<br>`region save`| *Version 1.15*<br>`region put us-southeast`<br>`region put us-ga us-southeast`<br>`region put us-ga-sw us-ga`<br>`region save` |

| Metro Atlanta (30-40 mile radius)| Augusta Area (Suggested) |
| :--- | :--- |
| *Version 1.16+*<br>`region def us-southeast us-ga us-ga-atl`<br>`region save`| *Version 1.16+*<br>`region def us-southeast us-ga us-ga-ags`<br>`region save` |
| *Version 1.15*<br>`region put us-southeast`<br>`region put us-ga us-southeast`<br>`region put us-ga-atl us-ga`<br>`region save`| *Version 1.15*<br>`region put us-southeast`<br>`region put us-ga us-southeast`<br>`region put us-ga-ags us-ga`<br>`region save`|

## Notes
- Remember you use regions to *keep traffic from leaking out* rather then to try to *route traffic out of a zone*. (regions are filters, not routes)
   - Most traffic is fine for *us-ga*, that should be your default scope for all repeater and companions
   - Scope channels or DM user to narrowly scoped regions as needed to keep traffic local
   - If you need to traverse your zone to another state, scope that channel or user to *us-southeast*. **You do not need to add that state's region!**
- Border cases can be handled via IATA type metro area like the proposed *us-ga-ags* for Augusta. Or a possible *us-tn-cha* for Chattanooga tri-state area. 
- Additional metro areas can be added where needed. But they are normally not relevant for smaller cities unless they are a *border* case like Augusta. 

