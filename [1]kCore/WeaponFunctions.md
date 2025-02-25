# Complete Weapons System Documentation

## Weapon System

### Weapon Configuration
```lua
{
    itemName = 'weapon_specialcarbine_mk2',
    label = 'Carbine Rifle',
    description = "Firearm",
    image = 'weapon_specialcarbine_mk2.png',
    maxStack = 1,
    unique = true,
    weight = 1,
    size = {
        width = 3,
        height = 2
    },
    rarity = 'rifle',
    shouldCloseInventory = true,
    type = 'weapon',
    ammoType = 'rifle_ammo'
}
```

### Weapon Events

#### kCore:equipWeapon
```lua
RegisterNetEvent('kCore:equipWeapon')
```
Features:
* Weapon equipping
* Ammo management
* Metadata handling

#### kCore:saveWeaponMetadata
```lua
RegisterNetEvent('kCore:saveWeaponMetadata')
```
* Saves weapon state
* Updates ammo count
* Persists metadata

#### kCore:removeWeapon
```lua
RegisterNetEvent('kCore:removeWeapon')
```
* Unequips weapon
* Cleans up weapon state

### Ammunition System

#### Ammunition Configuration
```lua
{
    itemName = 'rifle_ammo',
    label = 'Rifle Ammo',
    description = "Ammo",
    image = 'rifle_ammo.png',
    maxStack = 999,
    unique = false,
    weight = 1,
    size = {
        width = 1,
        height = 1
    },
    rarity = 'rifleAmmo',
    shouldCloseInventory = true,
    type = 'item'
}
```

#### Ammo Events

##### kCore:useAmmo
```lua
RegisterNetEvent('kCore:useAmmo')
```
Features:
* Ammo type validation
* Weapon compatibility check
* Metadata updates

##### kCore:updateWeaponAmmo
```lua
RegisterNetEvent('kCore:updateWeaponAmmo')
```
* Updates current weapon ammo
* Syncs with inventory

## Advanced Item Features

### Metadata System
* Custom data storage per item instance
* Persistent across inventory operations
* Updateable through `UpdateItemMetadata`

### Stack Management
* Automatic stack merging
* Maximum stack size enforcement
* Unique item handling

## Integration Examples

### Weapon Usage
```lua
-- Server-side weapon registration
exports.kCore:CreateUseableItem("weapon_pistol", function(source, item, slot)
    -- Equipment logic
    TriggerClientEvent('kCore:equipWeapon', source, {
        name = item.name,
        slot = slot,
        weaponHash = GetHashKey(string.upper(item.name)),
        ammoType = item.ammoType,
        metadata = item.metadata or { ammo = 0 }
    })
end)
```

### Ammo Usage
```lua
-- Client-side ammo usage
RegisterNetEvent('kCore:useAmmo', function(ammoData)
    if not currentWeapon then return end
    if currentWeapon.ammoType ~= ammoData.name then return end
    
    local currentAmmo = GetAmmoInPedWeapon(PlayerPedId(), currentWeapon.weaponHash)
    TriggerServerEvent('kCore:ammoUsed', ammoData, currentWeapon.slot, currentAmmo)
end)
```

### Implementation Guidelines

1. Weapon Handling
```lua
-- Always save weapon state before switching
TriggerEvent('kCore:saveWeaponMetadata', currentWeapon)
```

2. Ammo Management
```lua
-- Verify ammo compatibility
if currentWeapon.ammoType == ammoData.name then
    -- Process ammo usage
end
```

## Common Issues and Solutions

1. Image Loading
* Issue: Images not displaying
* Solution: Verify path in kcore/shared/items/itemImages/

2. Weapon Equipping
* Issue: Weapons not equipping
* Solution: Check weapon hash and ammo type configuration

3. Stack Handling
* Issue: Items not stacking
* Solution: Verify maxStack and unique properties

4. Metadata Persistence
* Issue: Lost metadata
* Solution: Ensure proper update event usage

## Error Handling

```lua
-- Example error checks
if not itemData then
    print("^1Item does not exist^7")
    return
end

if not Core.UsableItems[item.name] then
    print("^1Item is not usable^7")
    return
end
```

## Event Flow

### Weapon Usage Flow
```
Player Use -> CreateUseableItem Callback -> equipWeapon Event -> 
Weapon Equip -> Metadata Update -> Ammo Sync
```

### Item Usage Flow
```
Player Use -> UseItem Event -> Handler Callback -> 
Item Effect -> Inventory Update -> Client Sync
```


## Note all documentation right now will probably change as we make changes.