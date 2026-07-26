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

1. Download tg_lib and Extract it to your resouces folder
2. Add ``tg_lib`` in server.cfg before framework resource
```cfg
ensure tg_lib
ensure es_extended or qbx_core

```
3. Configuration

Add the following configuration template to your `config.lua` file. This includes core setups, framework adapters, and default starting economy balances:

```lua
Config = {}

-- ==========================================
-- 🔌 Framework & Economy Settings
-- ==========================================
Config.Framework = 'QBOX' -- Options: 'ESX', 'QBOX', 'STANDALONE'

Config.Economy = {
    DefaultStartingBalance = 5000, -- Default cash balance given to new players
    DefaultBankBalance = 15000,    -- Default bank balance given to new players
    CurrencySymbol = '$',          -- Currency display symbol
    AllowNegativeBalance = false,   -- Prevent player account balances from dropping below 0
}

-- ==========================================
-- 📦 Enabled Modules (Toggle true/false)
-- ==========================================
Config.Modules = {
    Ambulance = true,
    Clothing = true,
    Fuel = true,
    Inventory = true,
    Multicharacter = true,
    Weather = true,
    VehicleKeys = true,
    Target = true, -- Set to true to hook ox_target integrations
}

-- ==========================================
-- 🌐 Discord Webhook Logging
-- ==========================================
Config.Discord = {
    Webhook = "https://discord.com/api/webhooks/...", -- Replace with your Discord server webhook
    SystemLogs = true,
    EconomyLogs = true, -- Logs player balance transactions (deposits/withdrawals)
}
```

> [!IMPORTANT]
> Make sure `Config.Framework` matches the framework running on your server. If using QBOX, ensure `qbx_core` is fully loaded before `tg_lib` initialization.

