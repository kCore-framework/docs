# KCore Inventory System: How to Use Exports

This guide demonstrates how to trigger the exported functions from the KCore inventory system.

## Item Management

### CreateUseableItem
Create a usable item with a callback function:
```lua
exports.kCore:CreateUseableItem("itemName", function(source, item, slot)
    -- Your item use logic here
end)
```

### AddItem

Add an item to a player's inventory:

```lua
local success = exports.kCore:AddItem(source, "itemName", amount, metadata)
```

### RemoveItem

Remove an item from a player's inventory:

```lua
local success = exports.kCore:RemoveItem(source, "itemName", amount, slot)
```

### UpdateItemMetadata

Update the metadata of an item in a player's inventory:

```lua
local success = exports.kCore:UpdateItemMetadata(source, slot, metadata)
```

## Inventory Management

### CreateGroundInventory

Create a new ground inventory:

```lua
local groundInv = exports.kCore:CreateGroundInventory(source, "uniqueGroundId")
```

### GetInventoryById

Retrieve a ground inventory by its ID:

```lua
local inventory = exports.kCore:GetInventoryById("inventoryId")
```

### GetVehicleInventory

Retrieve or create a vehicle inventory:

```lua
local vehInv = exports.kCore:GetVehicleInventory("vehicleId", "Vehicle Label")
```

### MoveInventoryItem

Move an item from one inventory to another:

```lua
local success, updatedInventories = exports.kCore:MoveInventoryItem(source, itemData, sourceId, targetId)
```

### SplitInventoryItem

Split a stack of items in an inventory:

```lua
local success = exports.kCore:SplitInventoryItem(source, itemData, splitAmount)
```

## Vehicle Inventory

### SaveVehicleInventory

Save a vehicle's inventory to the database:

```lua
local success = exports.kCore:SaveVehicleInventory("vehicleId", inventoryData)
```

### LoadVehicleInventory

Load a vehicle's inventory from the database:

```lua
local inventoryData = exports.kCore:LoadVehicleInventory("vehicleId")
```

## Inventory Viewers

### AddInventoryViewer

Add a player as a viewer of an inventory:

```lua
exports.kCore:AddInventoryViewer("inventoryId", playerId)
```

### RemoveInventoryViewer

Remove a player as a viewer of an inventory:

```lua
exports.kCore:RemoveInventoryViewer("inventoryId", playerId)
```

### GetInventoryViewers

Retrieve all viewers of an inventory:

```lua
local viewers = exports.kCore:GetInventoryViewers("inventoryId")
```

## Note : May be subject to change in future references.