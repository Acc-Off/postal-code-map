# postal-code-map

A FiveM resource that replaces the in-game minimap with a custom map showing postal codes across the GTA V world.

## Credits

- Original map by **Virus_City** — https://forum.cfx.re/t/release-postal-code-map-minimap-new-improved-v1-3/147458  
- Minimap loader script based on **Roxwood Minimap** by RRixxles — https://github.com/RRixxles/Roxwood-Minimap/

## Known Issues

~~**Postal codes become blurry when zooming in (occurs when Graphics > Texture Quality is set to Normal or below).**~~ Fixed in Rev2.

## Changelog

### Rev3
Fixed a client crash that occurred when disconnecting and reconnecting to the server.

**Background:**  
On reconnect, FiveM reinitializes the streaming system via `RELOAD_MAP_STORE`. The original script called `AddMinimapOverlay("MINIMAP_LOADER.gfx")` immediately on resource start, which raced against this initialization and caused a crash in `gta-streaming-five.dll`.  
Analysis was performed using crash dumps (`CitizenFX_log`) which showed the following sequence every time:
```
Performing deferred RELOAD_MAP_STORE
Game finished loading!
Error: SendNetPacket called from multiple threads!  ← crash
gta-streaming-five.dll+70E67
```

**Changes (`client.lua`):**
- **Fixed `AddMinimapOverlay` call timing (direct cause of crash):** `AddMinimapOverlay("MINIMAP_LOADER.gfx")` races against `RELOAD_MAP_STORE` on reconnect and crashes `gta-streaming-five.dll`. Now waits until both `NetworkIsGameInProgress()` and `IsMinimapRendering()` return `true`, then adds an additional 3-second delay to ensure the streaming system has fully settled.
- **Added `onClientResourceStop` handler:** No cleanup was performed on resource stop. Now calls `RemoveMinimapOverlay`, `RemoveBlip` on all dummy blips, and `ResetMapZoomDataLevel`.
- **Fixed nil crash in `extendPauseMenuMapBounds`:** When `extraTiles` is empty, `drawnTiles[1]` is `nil`, causing a Lua script error. Added an `#extraTilesNames == 0` early-return guard.
- **Fixed radar zoom loop:** Changed `Citizen.Wait(1)` (1000 calls/sec) infinite loop to `Citizen.Wait(0)` (frame-synced) and added a `radarLoopActive` flag so the loop can be stopped cleanly from `onClientResourceStop`.

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
