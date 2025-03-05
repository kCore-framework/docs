# kCore Economy management documentation

All Infos you need about how kCore manages the economy including exports.

## General Information

Everything regarding money is handled server sided through the Core. You can either import the whole core and use the functions, or use the provided exports documented below. We recommend using the exports.

## updateMoney Event

Whenever the players money is updated, the `kCore:updateMoney` event is triggered both client and server sided to inform all resources about the changed money. You can use this event like this:

**Client**
```lua
AddEventHandler('kCore:updateMoney', function(Money, moneyType)
    if moneyType == 'cash' then
        print('Updated the players cash to ' .. Money.cash)
    else
        print('Updated the players bank account balance to ' .. Money.bank)
    end
end)
```

**Server**
```lua
AddEventHandler('kCore:updateMoney', function(source, Money, moneyType)
    if moneyType == 'cash' then
        print('Updated the players cash to for source ' .. source .. ' to ' .. Money.cash)
    else
        print('Updated the players bank for ' .. source .. ' account balance to ' .. Money.bank)
    end
end)
```

## updateAccountMoney Event

Whenever a company's money is updated, the `kCore:updateAccountMoney` event is triggered on the server sided to inform all resources about the changed balance. You can use this event like this:

```lua
AddEventHandler('kCore:updateAccountMoney', function(account, newBalance, oldBalance)
    local delta = newBalance - oldBalance
    print("The funds for " .. account .. "Changed by " .. delta)
end)
```

## Exports

Changing the players money is all handled through these exports.

### AddMoney

This export adds money to a player. Expected arguments are the source as integer, the amount as number and the money type as string. Returns wether or not the operation succeeded as bool. It can fail if the provided arguments are incomplete or the player is not online. Triggers a [player save](./PlayerObject.md#save)

```lua
local success = exports['kCore']:AddMoney(source, 100, "bank")
if not success then
    print('There has been an error!')
end
```

### RemoveMoney

This export adds money to a player. Expected arguments are the source as integer, the amount as number and the money type as string. Returns wether or not the operation succeeded as bool. It can fail if the provided arguments are incomplete, the player is not online or the player does not have enough money. Triggers a [player save](./PlayerObject.md#save)

```lua
local success = exports['kCore']:RemoveMoney(source, 100, "bank")
if not success then
    print('There has been an error!')
end
```


### GetMoney

Returns the players current money as table. Returns false if failed. Expects a source and a money type as arguments. Also refer to the [Player Object](./PlayerObject.md#money).

```lua
local money = GetPlayerMoney(source, "bank")
if not money then
    print('There has been an error!')
else
    print('The player has ' .. money.cash .. '$ in cash and ' .. money.bank .. '$ on the bank')
end
```

### TransferMoney

Transfers money from a source to a target. Expects source, target and amount as numbers. Returns a bool for wether or not the transfer was successful. It can fail if the source doesn't have enough money, arguments are missing/wrong, or one of the players isn't online. There are fallbacks implemented in case the removing the money from the source succeeds, but adding money to the target doesn't.

```lua
local success = exports['kCore']:TransferMoney(source, target, 100)
if not success then
    print('There has been an error!')
end
```

### GetAccountMoney

This export is used for company funds. It expects an account as argument and returns false if failed or the balance as number. Returns false if account was not provided or nil if the account wasn't found.

```lua
local company_funds = exports['kCore']:GetAccountMoney('police')
if not company_funds then
    print('There has been an error!')
else
    print('The police has ' .. company_funds .. '$ in their account')
end
```

### AddAccountMoney

Adds money to a company account. Expects the account name as string and the amount as number. Returns false in case it fails, or the new balance if successful.

```lua
local new_balance = exports['kCore']:AddAccountMoney('police', 100)
if not new_balance then
    print('There has been an error!')
else
    print('The police now has ' .. new_balance .. '$ in their bank account')
end
```

### RemoveAccountMoney

Removes money from a company account. Expects the account name as string and the amount as number. Returns false in case it fails, or the new balance if successful. Can fail if arguments are missing or the company doesn't have enough money.

```lua
local new_balance = exports['kCore']:RemoveAccountMoney('police', 100)
if not new_balance then
    print('There has been an error!')
else
    print('The police now has ' .. new_balance .. '$ in their bank account')
end
```