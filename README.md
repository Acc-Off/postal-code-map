# postal-code-map

A FiveM resource that replaces the in-game minimap with a custom map showing postal codes across the GTA V world.

## Credits

Originally published by **Virus_City** on the CFX community forums.  
Original release: https://forum.cfx.re/t/release-postal-code-map-minimap-new-improved-v1-3/147458

This repository is a repost of that resource.

## Known Issues

- **Postal codes become blurry when zooming in.**  
- **Custom minimap may not display correctly when in-game Graphics > Texture Quality is set to Normal or below.**  
  Setting Texture Quality to High or above resolves the issue.

## Installation

1. Copy the `postal-code-map` folder into your FiveM server's `resources` directory.
2. Add the following line to your `server.cfg`:
   ```
   ensure postal-code-map
   ```
3. Restart your server.

## Features

- Replaces the default minimap textures with a custom postal code map.
- Adjusts map zoom levels for better readability.
- Locks radar zoom to `1100` regardless of whether the player is on foot or in a vehicle.

## Files

| File | Description |
|------|-------------|
| `client.lua` | Sets map zoom data levels and locks radar zoom |
| `__resource.lua` | Resource manifest |
| `stream/` | Custom minimap texture files (`.ytd`, `.ydd`) |
