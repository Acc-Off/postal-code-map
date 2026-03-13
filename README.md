# postal-code-map

A FiveM resource that replaces the in-game minimap with a custom map showing postal codes across the GTA V world.

## Credits

- Original map by **Virus_City** — https://forum.cfx.re/t/release-postal-code-map-minimap-new-improved-v1-3/147458  
- Minimap loader script based on **Roxwood Minimap** by RRixxles — https://github.com/RRixxles/Roxwood-Minimap/

## Known Issues

~~**Postal codes become blurry when zooming in (occurs when Graphics > Texture Quality is set to Normal or below).**~~ Fixed in Rev2.

## Changelog

### Rev2
The known issue — postal codes appearing blurry when zooming in, which occurred when Graphics > Texture Quality was set to Normal or below — is resolved in this revision.

**Background:**  
A community fix for the blur issue was previously published at https://forum.cfx.re/t/free-release-postal-code-map-minimap-fixed/4882127, but it is no longer available.  
Separately, [Roxwood Minimap](https://github.com/RRixxles/Roxwood-Minimap/) was found to have no such issues.

**Changes:**  
- Replaced the original script with the one from **Roxwood Minimap** (`client.lua`, `config.lua`, `fxmanifest.lua`, `MINIMAP_LOADER.gfx`).  
- Re-processed the original `.ytd` texture files: set the mipmap level to `1` for all files where it was not already `1`. This eliminates the blur when zooming in and the rendering issue at Normal texture quality.  

## Installation

1. Copy the `postal-code-map` folder into your FiveM server's `resources` directory.
2. Add the following line to your `server.cfg`:
   ```
   ensure postal-code-map
   ```
3. Restart your server.

## Features

- Replaces the default minimap textures with a custom postal code map.
- Supports extra map tiles via `config.lua`.
- Loads the minimap via `MINIMAP_LOADER.gfx` for correct rendering at all texture quality settings.

## Files

| File | Description |
|------|-------------|
| `fxmanifest.lua` | Resource manifest (replaces `__resource.lua`) |
| `config.lua` | Extra tile configuration |
| `client.lua` | Minimap loader script (based on Roxwood Minimap) |
| `MINIMAP_LOADER.gfx` | Scaleform file used to load the minimap tiles |
| `stream/` | Custom minimap texture files (`.ytd`, `.ydd`) |
