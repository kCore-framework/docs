# Blip Info Documentation

## General Stuff

These are all the general parameters you have to provide for the blip info, not including the [components](#components)
```lua
blipInfo = {
    setTitle --[[string]] = "Mountain Drift Circuit",
    setType --[[int]] = 0,

    setTexture --[[table]] = { 
        -- either specify a texture dict and name
        dict --[[string]] = "kblipinfo", 
        name --[[string]] = "kcore",

        -- OR a DUI with url, width and height
        url --[[string]] = "https://i.pinimg.com/originals/89/5c/e7/895ce751ba0379700381d17a67086931.gif",
        width --[[int]] = 498,
        height --[[int]] = 427
    },

    setCashText --[[string]] = "$25,000",
    setApText --[[string]] = "2500",
    setRpText --[[string]] = "5000",
    components = {...} -- explained below
}
```
* setTitle | Main title of the blip shown in the info below the image/dui
* setType | There are 3 available types that change an icon in the top right corner: 0 - no icon, 1 - Rockstar Verified icon, 2 - Rockstar Created icon
* setTexture | This is used to either specify a texture dictionary including a texture name from a .ytd file (streamed or basegame), OR you can use a DUI to load an immage or a gif from the internet.
    * dict | The texture dictionary, the name of the .ytd file, without the file extension
    * name | Name of .dds file within that dictionary, again without the extension
    * url | The link to the DUI site you want to load
    * width | Width of that image in px
    * height | height of that image in px
* setCashText | This text is optional. It will be displayed next to a green dollar icon in the top right of your blip info
* setApText | This text is optional. It will be displayed next to an orange AP icon in the top right of your blip info
* setRpText | This text is optional. It will be displayed next to a blue RP icon in the top right of your blip info


## Components

There are 5 different component Types you can use for info that is shown below the main image and title. You can add as many of these components as you'd like in any order, they are rendered from top to bottom. Just add these to the `components` parameter.

### basic

Simple pair of Title on the left and value on the right.

```lua
{
    type --[[string]] = "basic",
    title --[[string]] = "Personal Best",
    value --[[string]] = "85,432 points",
},
```

### icon

Like the basic component, but adds an icon with an optional tick at the end of your value string.

```lua
{
    type --[[string]] = "icon",
    title --[[string]] = "Track Type",
    value --[[string]] = "Drift Course",
    iconIndex --[[int]] = 2,
    iconHudColor --[[int]] = 1,
    isTicked --[[bool]] = false
},
```
>Credit to jordanpkl for icon images

![Icon Index Reference](https://i.imgur.com/208Hia0.png)

### social

Rockstar Social Club style, could be used to show the creator for example. Optionally shows a rockstar icon next to the name and a crew tag like in GTA online

```lua
{
    type = "social",
    title = "Creator",
    value = "Kypos x Loki x Jordanpkl(EGG)",
    isSocialClubName = false, -- show rockstar icon next to the name
    crew = {
        tag = "KYPO",
        isPrivate = true, -- false triangle shape, true square shape
        isRockstar = true, -- shows R* icon
        lvl = 3, -- isPrivate needs to be true, values from 0-5, 0 is full bar, 5 is no bar
        lvlColor = "#00FFBF" -- Color of the lvl bar
    }
},
```

### divider

Pretty much just a white line. Can be used to divide the info up into different parts

```lua
{
    type = "divider"
},
```

### description

Basic description text. This one is multi-line and uses the whole width instead of just the side

```lua
{
    type = "description",
    value = "A challenging mountain course featuring tight hairpins and technical sections."
},
```
