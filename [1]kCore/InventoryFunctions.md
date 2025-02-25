# KCore Inventory Documentation

This document provides detailed information on how to use the KCore inventory functions externally.

## Table of Contents
1. [Item Configuration](#regular-items)
2. [Best Practices](#best-practices)
3. [Usable Items](#usable-items)
4. [Item Management](#item-management)
5. [Inventory Operations](#inventory-operations)
6. [Ground Inventories](#ground-inventories)
7. [Vehicle Inventories](#vehicle-inventories)
8. [Inventory Viewers](#inventory-viewers)


## Regular Items

### Shared Item Structure 
Items are defined in `Shared.Items` with the following structure:

```lua
{
    itemName = string,      -- Unique identifier
    label = string,         -- Display name
    description = string,   -- Item description
    image = string,         -- Image filename
    maxStack = number,      -- Maximum stack size
    unique = boolean,       -- Whether item can stack
    weight = number,        -- Item weight
    size = {               -- Grid size
        width = number,
        height = number
    },
    rarity = string,       -- Item rarity type
    shouldCloseInventory = boolean,  -- Close inventory on use
    type = string,         -- 'item' or 'weapon'
    ammoType = string      -- For weapons only
}
```

### Basic Item Configuration
```lua
{
    itemName = 'water',
    label = 'Water Bottle',
    description = "A nice water bottle",
    image = 'waterbottle.png',
    maxStack = 64,
    unique = false,
    weight = 1,
    size = {
        width = 1,
        height = 2
    },
    rarity = 'drink',
    shouldCloseInventory = true,
    type = 'item'
}
```
### Image Assets
* Location: `kcore/shared/items/itemImages/`
* Naming convention: Match `image` property in item configuration
* Supported formats: PNG recommended

## Best Practices

### Item Configuration
1. Image Naming
```lua
-- Match image property exactly
image = 'item_name.png'
```

2. Size Configuration
```lua
-- Consider inventory grid impact
size = {
    width = 1,  -- Minimum size
    height = 1
}
```

3. Stack Settings
```lua
-- For stackable items
maxStack = 64,
unique = false

-- For unique items
maxStack = 1,
unique = true
```

## Usable Items

### CreateUseableItem

Creates a usable item with a callback function.

```lua
Core.Functions.CreateUseableItem(itemName, callback)
```

- `itemName` (string): The name of the item
- `callback` (function): The function to be called when the item is used


Example:

```lua
Core.Functions.CreateUseableItem("water", function(source, item, slot)
    if Core.Functions.RemoveItem(source, item.name, 1, slot) then
        TriggerClientEvent('kCore:drink', source, item)
    end
end)
```

## Item Management

### AddItem

Adds an item to a player's inventory.

```lua
Core.Functions.AddItem(source, itemName, amount, metadata)
```

- `source` (number): The player's server ID
- `itemName` (string): The name of the item to add
- `amount` (number): The quantity of the item to add
- `metadata` (table, optional): Additional item information


Returns: `boolean` (true if successful, false otherwise)

Example:

```lua
local success = Core.Functions.AddItem(source, 'water', 1, {quality = 100})
if success then
    print("Added water to player's inventory")
end
```

### RemoveItem

Removes an item from a player's inventory.

```lua
Core.Functions.RemoveItem(source, itemName, amount, slot)
```
- `source` (number): The player's server ID
- `itemName` (string): The name of the item to remove
- `amount` (number): The quantity of the item to remove
- `slot` (table): Specific inventory slot to remove from, containing x and y coordinates



Returns: `boolean` (true if successful, false otherwise)

Example:

```lua
local slot = {x = 1, y = 1}
local success = Core.Functions.RemoveItem(source, 'water', 1, slot)
if success then
    print("Removed water from player's inventory")
end
```

### UpdateItemMetadata

Updates the metadata of an item in a player's inventory.

```lua
Core.Functions.UpdateItemMetadata(source, slot, metadata)
```

- `source` (number): The player's server ID
- `slot` (table): The slot containing the item to update
- `metadata` (table): The new metadata for the item


Returns: `boolean` (true if successful, false otherwise)

Example:

```lua
local slot = {x = 1, y = 1}
local newMetadata = {quality = 50}
local success = Core.Functions.UpdateItemMetadata(source, slot, newMetadata)
if success then
    print("Updated item metadata")
end
```

## Inventory Operations

### MoveInventoryItem

Moves an item from one inventory to another.

```lua
Core.Functions.MoveInventoryItem(source, item, sourceId, targetId)
```

- `source` (number): The player's server ID
- `item` (table): The item to move
- `sourceId` (string): The source inventory ID
- `targetId` (string): The target inventory ID


Returns: `boolean, table` (success status and updated inventory data)

Example:

```lua
local success, updatedInventories = Core.Functions.MoveInventoryItem(source, itemData, 'player', 'ground_123')
if success then
    print("Item moved successfully")
end
```

### SplitInventoryItem

Splits a stack of items in an inventory.

```lua
Core.Functions.SplitInventoryItem(source, item, splitAmount)
```

- `source` (number): The player's server ID
- `item` (table): The item to split
- `splitAmount` (number): The amount to split from the original stack


Returns: `boolean` (true if successful, false otherwise)

Example:

```lua
local success = Core.Functions.SplitInventoryItem(source, itemData, 5)
if success then
    print("Item stack split successfully")
end
```

## Ground Inventories

### CreateGroundInventory

Creates a new ground inventory.

```lua
Core.Functions.CreateGroundInventory(source, id)
```

- `source` (number): The player's server ID
- `id` (string): Unique identifier for the ground inventory


Returns: `table` (the created ground inventory)

Example:

```lua
local groundInv = Core.Functions.CreateGroundInventory(source, 'ground_123')
if groundInv then
    print("Ground inventory created")
end
```

### GetInventoryById

Retrieves a ground inventory by its ID.

```lua
Core.Functions.GetInventoryById(id)
```

- `id` (string): The ID of the ground inventory


Returns: `table` (the ground inventory, or nil if not found)

Example:

```lua
local groundInv = Core.Functions.GetInventoryById('ground_123')
if groundInv then
    print("Ground inventory found")
end
```

## Vehicle Inventories

### GetVehicleInventory

Retrieves or creates a vehicle inventory.

```lua
Core.Functions.GetVehicleInventory(inventoryId, label)
```

- `inventoryId` (string): The unique identifier for the vehicle inventory
- `label` (string): A label for the inventory (e.g., the vehicle model)


Returns: `table` (the vehicle inventory)

Example:

```lua
local vehInv = Core.Functions.GetVehicleInventory('veh_ABC123', 'Police Cruiser')
if vehInv then
    print("Vehicle inventory retrieved/created")
end
```

### SaveVehicleInventory

Saves a vehicle's inventory to the database.

```lua
Core.Functions.SaveVehicleInventory(invId, inventory)
```

- `invId` (string): The unique identifier for the vehicle inventory
- `inventory` (table): The inventory data to save


Returns: `boolean` (true if successful, false otherwise)

Example:

```lua
local success = Core.Functions.SaveVehicleInventory('veh_ABC123', vehInventoryData)
if success then
    print("Vehicle inventory saved to database")
end
```

### LoadVehicleInventory

Loads a vehicle's inventory from the database.

```lua
Core.Functions.LoadVehicleInventory(invId)
```

- `invId` (string): The unique identifier for the vehicle inventory


Returns: `table` (the loaded inventory data, or nil if not found)

Example:

```lua
local loadedInv = Core.Functions.LoadVehicleInventory('veh_ABC123')
if loadedInv then
    print("Vehicle inventory loaded from database")
end
```

## Inventory Viewers

### AddInventoryViewer

Adds a player as a viewer of an inventory.

```lua
Core.Functions.AddInventoryViewer(inventoryId, playerId)
```

- `inventoryId` (string): The ID of the inventory
- `playerId` (number): The server ID of the player to add as a viewer


Example:

```lua
Core.Functions.AddInventoryViewer('ground_123', source)
```

### RemoveInventoryViewer

Removes a player as a viewer of an inventory.

```lua
Core.Functions.RemoveInventoryViewer(inventoryId, playerId)
```

- `inventoryId` (string): The ID of the inventory
- `playerId` (number): The server ID of the player to remove as a viewer


Example:

```lua
Core.Functions.RemoveInventoryViewer('ground_123', source)
```

### GetInventoryViewers

Retrieves all viewers of an inventory.

```lua
Core.Functions.GetInventoryViewers(inventoryId)
```

- `inventoryId` (string): The ID of the inventory


Returns: `table` (a table of player IDs viewing the inventory)

Example:

```lua
local viewers = Core.Functions.GetInventoryViewers('ground_123')
for playerId, _ in pairs(viewers) do
    print("Player " .. playerId .. " is viewing the inventory")
end
```

## Events

The inventory system also includes several events that can be used:

- `kCore:useItem`: Triggered when a player uses an item
- `kCore:updateItemMetadata`: Triggered when item metadata is updated


These events are handled internally but can be utilized or extended as needed in your server scripts.