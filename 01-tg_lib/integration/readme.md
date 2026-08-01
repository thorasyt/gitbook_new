# Resource Integrations

`tg_lib` is designed to be highly interoperable and modular. It includes automatic integration wrappers for popular third-party FiveM resources (such as clothing systems, target systems, inventories, and fuel scripts).

---

### Integration Architecture

Rather than creating separate folders for each integration, `tg_lib` groups them directly by side (Client vs. Server) since the framework dynamically detects and loads the correct adapters in the background.

Explore the APIs for integrations:

*   [🔌 Client Side Integrations](client.md) — Fuel, Target, Inventory, and Vehicle Keys client wrappers.
*   [⚙️ Server Side Integrations](server.md) — Ambulance, Clothing shops, Multicharacter, Weather sync, and Server-side inventory stashes.
