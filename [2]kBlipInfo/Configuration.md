# Configuration Documentation

You have 2 ways of setting up Blips, either creating the blip including through the config.lua file, or through exports from other scripts creating the blips.

## Using the Exports

### Export
```lua
exports['kBlipInfo']:UpdateBlipInfo(blip, info)
```
This is the export you need to call. blip should be a valid blip handle and info should be a table of options. The options are explained below.

### Example

An example from this can also be found in the client/example.lua file. You could do something like this:
```lua
local blip = AddBlipForCoord(-123.6383, 562.4001, 196.0399)
SetBlipSprite(blip, 545)
BeginTextCommandSetBlipName("STRING")
AddTextComponentString("Mountain Drift Circuit")
EndTextCommandSetBlipName(blip)

local info = {}

exports['kBlipInfo']:UpdateBlipInfo(blip, info)
```

All options you have for the info can be found on the [blip info page](blipinfo.md)

## Using the Config


First you'll have to set up general blip settings like the position. These are the parameters you need to provide:
```lua
position --[[Vector3]] = vector3(-123.6383, 562.4001, 196.0399),
blipId --[[int]] = 545,
blipColor --[[int]] = 47,
displayType --[[int]] = 4,
scale --[[float]] = 1.0,
shortRange --[[bool]] = false,
label --[[string]] = "Mountain Drift Circuit",
```
* position | Location of the blip as Vector3.
* blipId | The ID of the blip chooses what sprite is shown on the minimap. All valid IDs can be found in the [fivem documentation](https://docs.fivem.net/docs/game-references/blips/).
* blipColor | The GTA Blip color code for the color you wish to use. This is NOT an RGB or hex value. The available color IDs can be found in the [fivem documentation](https://docs.fivem.net/docs/game-references/blips/#blip-colors).
* displayType | This changes how the blip is visible, for example only on the pause menu map and not on the minimap. All the display types are [documented](https://docs.fivem.net/natives/?_0x9029B2F3DA924928).
* scale | Simply the size of the blip on the minimap, 1.0 is the normal scale. Keep in mind, this has to be a float.
* shortRange | If you set this boolean to true, the blip is only visible on the minimap when you are close to it. Otherwise the blip is always displayed and sticks to the borders of your minimap, even when far away.
* label | The name shown on the map

##

Now just add the options for the blip information and you're done! You can add as many more blips as you'd like. All blips from this Config are created by the script as soon as the player joins.
