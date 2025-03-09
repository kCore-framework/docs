# KCore Player Object: What is included

This page lists everything that comes with the kCore Player object, including functions and values. Many things included in the main Player object can be received better through exports targeting only that one element, reducing traffic. The export to get the element is always mentioned. It is generally recommended to use the export for the specific thing you want, instead of getting the whole player or core.

## Getting the Player Object

For getting the Player in the first place, you have two options.

1. Through the core functions:
```lua
local kCore = exports['kCore']:GetCore()
local Player = kCore.Functions.GetPlayer(source)
```
2. Through the direct export:
```lua
local Player = exports['kCore']:GetPlayer(source)
```
We recommend using method 2 to prevent importing a bunch of stuff that you'll never use. We use method 2 in the examples in this documentation but you can of course use your preferred way. If possible, you should use the specific exports.

## source

The players source, aka server ID. Quite useless in this example but might be useful when looping through all players or whatever.
<br>
No export available.

```lua
local Player = exports['kCore']:GetPlayer(source)
local src = Player.source
```

## citizenid

The Citizen ID that is assigned to the player by kCore.
<br>
No export available at this time.

```lua
local Player = exports['kCore']:GetPlayer(source)
local citizenid = Player.citizenid
```

## Money

The players current money, both for their bank account as well as the cash in their inventory. The Money is returned as a table with two keys, money and cash, both having a number value representing the players money.
<br>
Can also be received through `exports['kCore']:GetMoney(source, type)` where type is either cash or bank, returning the value as number or false if failed.

```lua
local Player = exports['kCore']:GetPlayer(source)
local Money = Player.Money

local cash = Money.cash
local bank = Money.bank

-- returned like this:
Player.Money = {
    cash = 0,
    bank = 0
}
```

## Job

The players Job as a table, including the jobs name, label, players grade, grade label and salary.
<br>
Can also be received through `exports['kCore']:GetPlayerJob(source)`.

```lua
local Player = exports['kCore']:GetPlayer(source)
local Job = Player.Job

-- returned like this:
Player.Job = {
    name = "unemployed", -- name used to identify the job
    label = "Unemployed", -- label shown in menus or hud
    grade = 0, -- grade/level starting at 0
    grade_label = "Unemployed", -- label of the grade shown in menus or hud
    salary = 0 -- hourly salary
}
```

## position

The players position as table **(not vector!)**. This position is updated on each player save and is primarily used to spawn the player at the same position the disconnected. This should _**NOT**_ be used to accurately get the players position, use the [FiveM Native](https://docs.fivem.net/natives/?_0x1647F1CB) `GetEntityCoords(ped)` instead.
<br>
No export available.

```lua
local Player = exports['kCore']:GetPlayer(source)
local position = Player.position

-- returned like this:
Player.position = {
    x = -1042.0345,
    y = -2744.7874,
    z = 20.3594,
    heading = 325.2877
}
```

## Name

The player's character first and last name in a table.
<br>
No export available.

```lua
local Player = exports['kCore']:GetPlayer(source)
local Name = Player.Name

local fullName = Name.first_name .. " " .. Name.last_name

-- returned like this:
Player.Name = {
    first_name = "John",
    last_name = "Doe"
}
```

## Stats

The Players current hunger and thirst in a table.
<br>
No export available.

```lua
local Player = exports['kCore']:GetPlayer(source)
local Stats = Player.Stats

-- returned like this:
Player.Stats = {
    hunger = 100,
    thirst = 100
}
```

## Appearance

The Players Skin in a table, including clothing, ped model, genetics, face features and head overlays. Irrelevant until kClothing release.
<br>
No export available.

```lua
local Player = exports['kCore']:GetPlayer(source)
local Skin = Player.Appearance

-- returned like this:
Player.Appearance = {
    model = "mp_m_freemode_01", -- ped model

    -- will be properly documented after kClothing release:
    clothing = {},
    genetics = {},
    faceFeatures = {},
    headOverlays = {}
}
```

## Inventory

The Players Inventory as a table. Irrelevant until kInventory release.
<br>
No export available.

```lua
local Player = exports['kCore']:GetPlayer(source)
local inv = Player.Inventory

-- returned like this:
Player.Inventory = {
    maxWeight = 100,
    rows = 10,
    columns = 10,
    items = {}
}
```

## Functions

This section contains a bunch of Functions included in the Player object.

### Save

This function is used to save the players current data. Should in most cases be handled automatically by the core, but could be called manually if needed.
<br>
Triggers the `kCore:updateData` Client Event after saving.
<br>
[Export available](./Exports.md#saveplayerdata)

```lua
local Player = exports['kCore']:GetPlayer(source)

Player.Functions.Save()
```

### UpdateMoney

This function sets the players money to the passed amount. Should not be used anymore, as it includes no checks whatsoever. Refer to the [Economy Documentation](./Economy.md). Returns true or false for success or failed.
<br>
Exports listed in the Economy documentation.

```lua
local Player = exports['kCore']:GetPlayer(source)

local moneyType = "cash"
local amount = 100
local success = Player.UpdateMoney(moneyType, amount) -- sets the players cash to 100
if not success then
    print('Failed to update money!')
end
```

### UpdateJob

This function sets the players Job to the provided job and grade. Should not be used anymore, refer to the [jobs documentation](./Jobs.md). Triggers a [save](#save). Returns true if succeeded, false if job or grade doesn't exist.
<br>
Exports listed in Jobs documentation.

```lua
local Player = exports['kCore']:GetPlayer(source)

local job = "unemployed" -- job name, not label
local grade = 0

local success = Player.UpdateJob(job, grade) -- sets the players job to unemployed, grade 0
if not success then
    print('Something went wrong!')
end
```

### UpdateAppearance

This function updates the players saved Appearance, but doesn't apply it. Useless until kClothing release. Accepts a table as argument with the same structure as returned in [Player.Appearance](#appearance), but has fallbacks in case anything is not included. Always returns true.
<br>
Exports will be added with kClothing.

```lua
local Player = exports['kCore']:GetPlayer(source)

local Appearance = {} -- refer to Player.Appearance
Player.UpdateAppearance(Appearance)
```

### GetAppearance

This function just returns Player.Appearance. Accepts no arguments.
<br>
No Export available.

```lua
local Player = exports['kCore']:GetPlayer(source)

local appearance = Player.GetAppearance()

-- same as:
local appearance = Player.Appearance
```
Refer to [Player.Appearance](#appearance) for format and more details.


### UpdateInventory

This functions updates the players inventory and saves it to the database. Should not be used as it is still highly experimental and will be reworked. This function expects the full inventory object as argument, for just adding/removing items you should refer to the Inventory [Functions](./InventoryFunctions.md) and [Exports](./InventoryExports.md). Also refer to [Player.Inventory](#inventory). Always returns true.
<br>
Exports documented in [Inventory Exports](./InventoryExports.md).

```lua
local Player = exports['kCore']:GetPlayer(source)

local Inventory = {} -- refer to inventory documentation
Player.UpdateInventory(Inventory)
```
