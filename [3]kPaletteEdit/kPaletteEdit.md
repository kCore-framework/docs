# Palette Edit

A FiveM resource for managing head blend palette colors, allowing customization of character appearance through RGB color values across multiple palette indices.

## Installation

1. Download the kPaletteEdit resource
2. Place it in your server's resources directory
3. Add to your server.cfg:
```cfg
ensure kPaletteEdit
```
## Configuration

Configure default palette color in server.cfg:
```cfg
# Default color is mint green (3, 252, 194)
set palette_default_red "3"      # Default: 3
set palette_default_green "252"  # Default: 252
set palette_default_blue "194"   # Default: 194
```
## Client Exports
### SetHeadBlendPaletteColor
Set a single palette color

```lua
-- Parameters:
-- paletteIndex: number (0-3)
-- red: number (0-255)
-- green: number (0-255)
-- blue: number (0-255)
exports['kPaletteEdit']:SetHeadBlendPaletteColor(paletteIndex, red, green, blue)

-- Example:
exports['kPaletteEdit']:SetHeadBlendPaletteColor(0, 255, 0, 0) -- Set first palette to red
```
### SetHeadBlendPaletteColors
Set multiple palette colors at once:

```lua
-- Parameters:
-- colors: table of arrays, each containing [index, red, green, blue]
exports['kPaletteEdit']:SetHeadBlendPaletteColors(colors)

-- Example:
local colors = {
    {0, 255, 0, 0},    -- Red
    {1, 0, 255, 0},    -- Green
    {2, 0, 0, 255},    -- Blue
    {3, 255, 255, 0}   -- Yellow
}
exports['kPaletteEdit']:SetHeadBlendPaletteColors(colors)
```

### CheckAndApplyColors
Force reapplication of current palette colors:

```lua
exports['kPaletteEdit']:CheckAndApplyColors()
```
### InitializePaletteColors
Request and apply initial palette state:

```lua
exports['kPaletteEdit']:InitializePaletteColors()
```
## Server Exports
### SetHeadBlendPaletteColor
Set a single palette color for a specific player:

```lua
-- Parameters:
-- source: number (player id)
-- paletteIndex: number (0-3)
-- red: number (0-255)
-- green: number (0-255)
-- blue: number (0-255)
exports['kPaletteEdit']:SetHeadBlendPaletteColor(source, paletteIndex, red, green, blue)
```

### SetHeadBlendPaletteColors
Set multiple palette colors for a specific player:

```lua
-- Parameters:
-- source: number (player id)
-- colors: table of arrays, each containing [index, red, green, blue]
exports['kPaletteEdit']:SetHeadBlendPaletteColors(source, colors)
```

## Framework Integration

### KCore
```lua
-- kCore exmaple
RegisterNetEvent('kCore:loadPlayer', function()
    exports['kPaletteEdit']:InitializePaletteColors()
end)
```
### QBCore
```lua
-- QBCore example:
RegisterNetEvent('QBCore:Client:OnPlayerLoaded', function()
    exports['kPaletteEdit']:InitializePaletteColors()
end)
RegisterNetEvent('qb-clothing:client:loadPlayerClothing', function()
    exports['kPaletteEdit']:CheckAndApplyColors()
end)
```

### ESX
```lua
-- ESX example:
RegisterNetEvent('esx:playerLoaded', function()
    exports['kPaletteEdit']:InitializePaletteColors()
end)
```

## Technical Details
- Uses state bags for network synchronization
- Automatic default color handling

## Events

The resource handles these events internally:
- `playerJoining`: Initialize player palette data
- `playerDropped`: Cleanup player data
- `headBlend:setPaletteColor`: Handle individual color updates
- `headBlend:setPaletteColors`: Handle bulk color updates
- `headBlend:requestInitialState`: Request current palette state

## Notes

- All palette indices must be between 0 and 3
- RGB values must be between 0 and 255
- Default colors are applied automatically when no custom color is set
- Resource automatically handles network state synchronization