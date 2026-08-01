# Module Architecture

All `tg_lib` modules reside in the `tg_lib/modules/` directory. Each module is housed in its own folder and follows a clean structural separation depending on whether its logic runs on the server, client, or both.

---

### Folder & File Structure

```
tg_lib/modules/
├── aceperms/
│   └── server.lua          # Server-side dynamic ACE permissions
├── callback/
│   ├── client.lua          # Client-side callbacks
│   └── server.lua          # Server-side callbacks
├── command/
│   └── server.lua          # Server-side command registration with validation
├── database/
│   └── server.lua          # Server-side database MySQL wrappers
├── discord/
│   ├── client.lua          # Client-side Discord avatar retrieval
│   └── server.lua          # Server-side Discord API integration
├── keybinds/
│   └── client.lua          # Client-side RegisterKeyMapping keybinds
├── playerstatus/
│   └── server.lua          # Server-side uptime and analytics tracking
├── points/
│   └── client.lua          # Client-side spatial grid coordinates tracking
├── vehiclespawn/
│   ├── client.lua          # Client-side vehicle mod & properties parsing
│   └── server.lua          # Server-side vehicle entity spawning and persistence
└── zonecreator/
    └── client.lua          # Client-side 3D boundaries collision checking (box, sphere, etc.)
```

### How Modules Work

1. **Separation of Concerns**: Logic is divided into `server.lua` (loaded only on the server) and `client.lua` (loaded only on the client). This isolates client UI/inputs from server database/API calls.
2. **Auto-Loading**: The framework's `fxmanifest.lua` automatically scans and loads files under these folders:
   - `modules/**/client.lua` is loaded under client scripts.
   - `modules/**/server.lua` is loaded under server scripts.
3. **The Global `TG` Object**: During initialization, each module registers itself directly to the global `TG` table (e.g., `TG.callback = callback`, `TG.zones = Zones`).
4. **Proxy Wrapper**: When a third-party resource references the library by importing `@tg_lib/init.lua` in its manifest, a metatable wrapper proxy routes all calls back to the running `tg_lib` export. This exposes the module APIs seamlessly.

---

### Core Modules List

Explore the documentation for each specific module's side:

*   **🏋️ Ace Permissions**: [Server Side](aceperms/server.md) | [Client Side](aceperms/client.md)
*   **📞 Callbacks**: [Server Side](callback/server.md) | [Client Side](callback/client.md)
*   **💬 Commands**: [Server Side](command/server.md)
*   **📈 Database**: [Server Side](database/server.md)
*   **📟 Discord**: [Server Side](discord/server.md) | [Client Side](discord/client.md)
*   **🎹 Keybinds**: [Client Side](keybinds/client.md)
*   **📊 Player Status**: [Server Side](playerstatus/server.md)
*   **🔺 Points**: [Client Side](points/client.md)
*   **🚘 Vehicle Spawning**: [Server Side](vehiclespawn/server.md) | [Client Side](vehiclespawn/client.md)
*   **⭕ Zones**: [Client Side](zonecreator/client.md)

### Global & Utility Modules

*   **🐛 Debug**: [Shared](debug/shared.md)
*   **👤 Get Player Identifier**: [Shared](getidentifier/shared.md)
*   **👥 Nearby Players**: [Shared](nearbyplayers/shared.md)
*   **👤 Player Name**: [Shared](playername/shared.md)
*   **⚡ Random Generator**: [Shared](random/shared.md)
*   **📦 Require & JSON Helpers**: [Shared](require/shared.md)
*   **🏁 Resource State Check**: [Shared](resourcestate/shared.md)
*   **⌛ Wait For Callback**: [Shared](waitfor/shared.md)
