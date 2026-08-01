# Georgia Meshcore Region Information

## Design Objectives
### Focus on traffic management
   1. Core issue is to manage flooding leakage that are creating issues with adjacent
   zones and states

### Keep it as simple as possible

   1. Intuitive zones, no map or rules needed
   2. Minimal number of zones to allow interstate traffic and restrict to local
   3. Minimal of commands to execute on repeaters to set up regions

### Align to defacto standard 
   3. us-xx-yy* is the international standard and adopted by many US states

### Avoid future name duplication with other states
   4. The us-xx-yy avoids potential name clashes with other countries, or subregions 
   in adjacent states. 

## Structure

```
us-southeast
   us-ga
      us-ga-xx (quadrant zone) or us-ga-xxx (iata metro area)
```

## Timeframe

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
