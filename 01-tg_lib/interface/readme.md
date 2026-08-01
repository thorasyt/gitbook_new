# User Interface Overview

`tg_lib` features a rich collection of HTML5-based UI components (built using HTML, Javascript, and Vanilla CSS styles) designed to provide developers with responsive, premium, and easy-to-use HUD and dialog controls.

---

### Folder & File Structure

All UI components are documented in the folder-wise layout under `tg_lib/interface/`:

```
tg_lib/interface/
├── alert/
│   └── client.md           # Client-side Alert dialog API
├── announce/
│   ├── client.md           # Client-side Announcement HUD API
│   └── server.md           # Server-side Announcement triggers
├── context/
│   └── client.md           # Client-side Custom Context menus
├── input/
│   └── client.md           # Client-side Input Form dialogs
├── menu/
│   └── client.md           # Client-side List menus HUD API
├── noti/
│   ├── client.md           # Client-side Toast notifications API
│   └── server.md           # Server-side Toast triggers
└── texui/
    └── client.md           # Client-side HUD TextUI indicators
```

### How UI Components Work

1. **HTML/NUI Integration**: All screens are rendered via NUI (`ui/dist/index.html`) using HTML messages sent from client scripts using FiveM's native `SendNUIMessage` API.
2. **NUI Focus Management**: Components dynamically manage keyboard and mouse cursor input state through the library's internal `nui.lua` wrapper, ensuring game controls are disabled/enabled smoothly as menus open and close.
3. **Themes & Aesthetics**: The UI styles are tailorable from the library's built-in debug customization commands (like `/tg_lib_ui` on the server).

---

### UI Components List

Explore the APIs for each user interface module:

*   **⚠️ Alert Dialogs (`alert`)**: [Client Side](alert/client.md)
*   **📣 Announcements (`announce`)**: [Server Side](announce/server.md) | [Client Side](announce/client.md)
*   **📋 Context Menus (`context`)**: [Client Side](context/client.md)
*   **📝 Input Forms (`input`)**: [Client Side](input/client.md)
*   **📂 List Menus (`menu`)**: [Client Side](menu/client.md)
*   **🔔 Notifications (`noti`)**: [Server Side](noti/server.md) | [Client Side](noti/client.md)
*   **💬 TextUI Indicators (`texui`)**: [Client Side](texui/client.md)
