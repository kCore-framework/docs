# kCore Exports

This page lists all exports currently available in kCore. In many places, exports can be used to directly use the functions you need, instead of importing the entire core. It is generally recommended to use these exports if possible instead of importing the whole core.

## Client-Side

### TriggerServerCallback

Triggers a Server Callback. Expects the callback name and a callback function as arguments, as well as any optional parameters for the callback.

```lua
exports['kCore']:TriggerServerCallback('my_resource:my_callback', function(args)
    print('Callback triggered!')
end, args)
```

### GetCore

Returns the Core client side.

```lua
local Core = exports['kCore']:GetCore()
```

## Server-Side

### RegisterServerCallback

Registers a Server Callback. Expects a callback name as event and a callback function.

```lua
exports['kCore']:RegisterServerCallback("my_resource:my_callback", function(src, cb)
    -- Do your checks

    cb()
end)
```

### AddMoney

Refer to the [Economy Documentation](./Economy.md#addmoney)

### RemoveMoney

Refer to the [Economy Documentation](./Economy.md#removemoney)

### GetMoney

Refer to the [Economy Documentation](./Economy.md#getmoney)

### TransferMoney

Refer to the [Economy Documentation](./Economy.md#transfermoney)

### GetAccountMoney

Refer to the [Economy Documentation](./Economy.md#getaccountmoney)

### AddAccountMoney

Refer to the [Economy Documentation](./Economy.md#addaccountmoney)

### RemoveAccountMoney

Refer to the [Economy Documentation](./Economy.md#removeaccountmoney)

### CreateUsableItem

Refer to the [Inventory Documentation](./InventoryExports.md#createuseableitem)

### AddItem

Refer to the [Inventory Documentation](./InventoryExports.md#additem)

### GetVehicleInventory

Refer to the [Inventory Documentation](./InventoryExports.md#getvehicleinventory)

### RemoveItem

Refer to the [Inventory Documentation](./InventoryExports.md#removeitem)

### UpdateItemMetadata

Refer to the [Inventory Documentation](./InventoryExports.md#updateitemmetadata)

### CreateGroundInventory

Refer to the [Inventory Documentation](./InventoryExports.md#creategroundinventory)

### GetInventoryById

Refer to the [Inventory Documentation](./InventoryExports.md#getinventorybyid)

### MoveInventoryItem

Refer to the [Inventory Documentation](./InventoryExports.md#moveinventoryitem)

### SplitInventoryItem

Refer to the [Inventory Documentation](./InventoryExports.md#splitinventoryitem)

### SaveVehicleInventory

Refer to the [Inventory Documentation](./InventoryExports.md#savevehicleinventory)

### LoadVehicleInventory

Refer to the [Inventory Documentation](./InventoryExports.md#loadvehicleinventory)

### AddInvetoryViewer

Refer to the [Inventory Documentation](./InventoryExports.md#addinventoryviewer)

### RemoveInventoryViewer

Refer to the [Inventory Documentation](./InventoryExports.md#removeinventoryviewer)

### GetInventoryViewers

Refer to the [Inventory Documentation](./InventoryExports.md#getinventoryviewers)

### CreateJob

Refer to the [Jobs Documentation](./Jobs.md)

### SetPlayerJob

Refer to the [Jobs Documentation](./Jobs.md)

### GetJob

Refer to the [Jobs Documentation](./Jobs.md)

### GetPlayerJob

Refer to the [Jobs Documentation](./Jobs.md)


### GetAllJobs

Refer to the [Jobs Documentation](./Jobs.md)

### GetCore

Returns the entire Core with all functions. You should rather use specific exports instead.

```lua
local core = exports['kCore']:GetCore()
```

### GetCharacterSlots

Get all character slots for a specific player. Expects the identifier as string and a callback function that is called with the result.

```lua
exports['kCore']:GetCharacterSlots(citizenid, function(data)
    -- data looks like this:
    data = {
        characterSlots = {
            {
                id = 0,
                slot = 1,

                -- for these refer to the PlayerObject documentation
                citizenid = "users_citizenid",
                Name = {},
                Stats = {},
                Appearance = {},
                Money = {},
                Job = {},
                position = {}
            }
        },
        maxSlots = 3,
        autoload = false,
    }
end)
```

### CreateCharacter

Manually creates a character. Expected arguments are citizenid, slot, char data, source and a callback function.

```lua
local citizenid = "123"
local slot = 3 -- use a slot that is not already in use
local data = {
    sex = "male",
    firstName = "John",
    lastName = "Doe"
}

exports['kCore']:CreateCharacter(citizenid, slot, source, function(success, citizenid)
    if not success then
        print('There was an error creating the character')
    end
end)
```

### GetPlayer

Gets the entire player object for a specific source. Refer to the [Player Object Documentation](./PlayerObject.md) for details.

```lua
local Player = exports['kCore']:GetPlayer(source)
```

### GetPlayerByCitizenId

Gets the entire player object for a specific player by their citizenid. Refer to the [Player Object Documentation](./PlayerObject.md) for details.

```lua
local Player = exports['kCore']:GetPlayerByCitizenId(citizenid)
```

### GetPlayers

Gets all players with their full player object. That's a lot of data so only use this if really necessary. efer to the [Player Object Documentation](./PlayerObject.md) for details.

```lua
local Players = exports['kCore']:GetPlayers()
```

### SavePlayerData

Saves the players data, should in most cases be done automatically by the core without issues. Same as calling the [Player.Functions.Save function](./PlayerObject.md#save)

```lua
local success = exports['kCore']:SavePlayerData(source)
```

### UpdatePlayerAppearance

This function updates the players saved Appearance, but doesn't apply it. Useless until kClothing release. Accepts a table as argument with the same structure as returned in [Player.Appearance](./PlayerObject.md#appearance), but has fallbacks in case anything is not included. Returns false if the player was not found by provided source. Otherwise same as [Player.Functions.UpdateAppearance](./PlayerObject.md#updateappearance)

```lua
local success = exports['kCore']:UpdatePlayerAppearance(source, data)
```

### SelectCharacter

Selects a character for a player. Expects a currently unused id, a slot, the source and a currently unused callback function.

```lua
exports['kCore']:SelectCharacter(nil, slot, source, nil)
```

### HasPermission

Checks wether or not a source has the passed permission. Returns true or false.

```lua
local allowed = exports['kCore']:HasPermission(source, "MyAcePerm")
if not allowed then
    print('Player is not allowed!')
end
```

### GetJobs

Refer to the [Job Documentation](./Jobs.md).
