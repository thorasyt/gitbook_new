# tg_lib
Advance lib which has so much module to handle activity
---
## Features

- 🎞️ Framework Modules (ESX, QBOX)
- 🚑 Ambulance Job Module
- 👗 Clothing Module
- ⛽ Fuel Module
- 📦 Inventory Module
- ♂️ Multicharacter Module
- 👁️ Target Module (ox_target)
- 🔑 VehicleKeys Module
- ☁️ Weather Module
- 🌐 Interface Module (inputdialog, menu, contextmenu, textui, etc...)
- 🏋️ Ace Permisson Module
- 📞 Callback Module
- 💬 Command Module
- 📈 Database Module
- 📟 Discord Module
- 🎹 Keybind Module
- 🔺 Point Module
- 🚘 Vehicle Handle Module
- ⭕ Zone Module

### Installation

##### 1. Download tg_lib and Extract it to your resouces folder
##### 2. Add ``tg_lib`` in server.cfg before framework resource
```cfg
ensure tg_lib
ensure es_extended or qbx_core

```
##### 3. Configuration

Add the following configuration template to your `config.lua` file. This includes core setups, framework adapters, and default starting economy balances:

```lua
return {
    DISCORD_API_URL = "https://discord.com/api/v9", -- don't change if you don't know what you are doing

    botToken = '', -- bot token which can achive from discord developer portal
    guildId = '', -- guild id which the bot need to be
    databaseString = {
        interfaice_wait =
            'SELECT * FROM tg_lib WHERE identifier = ?',
    
        interfaice_create =
            'INSERT INTO tg_lib (identifier) VALUES (?)',
    
        interfaice_update =
            'UPDATE tg_lib SET notification_settings = ?, listmenu_position = ?, contextmenu_position = ?, textui_settings = ? WHERE identifier = ?',
    
        istableExist =
            'SELECT COUNT(*) AS count FROM information_schema.tables WHERE table_schema = DATABASE() AND table_name = ?',
        
        banned_wait =
            'SELECT * FROM tg_banned_players WHERE license = ?',
    },
    usersAsAdmin = {
        --['discord:869838160841019402'] = 'admin'
        --Addition Admin or ace permission is require
    }
}
```

> [!IMPORTANT]
> Make sure `botToken` need to set or else some of the function not work.

##### 4. Add to server.cfg
```cfg
add_ace resource.tg_lib command allow
```

### How to use
To enable the library inside of your resource just add @tg_lib/init.lua as a shared_script in your fxmanifest.lua file.

```lua
shared_scripts {
    '@tg_lib/init.lua',
}
```
```lua
shared_script '@tg_lib/init.lua'
```