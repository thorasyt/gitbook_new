# tg-admin — Administration Panel & Management System

`tg-admin` is a powerful, modern administration tool for FiveM roleplay servers. Built with a React-based NUI panel and integrated directly with `tg_lib`, it offers real-time server tracking, player monitoring, ticket management, prop spawning, and vehicle modification controls.

## Features Checklist

### 1. System & Server Monitoring
* **Real-Time System Stats**: Displays active CPU usage, RAM usage, and server ping metrics for the administrator.
* **Server Information Board**: Tracks server uptime, resource count, artifact build version, and OneSync state.
* **Online Player Graphing**: Renders a dynamic line graph tracking online player counts over a 6-hour timeline.
* **Recent Activity Feed**: Provides an audit trail of admin actions (e.g., set weather, clear area, bans/kicks) directly in the menu.

### 2. Player Management (Interactive Directory)
* **Detailed Info Cards**: View targeted player stats including current Health, Armor, Cash/Bank balances, Job, Gang, and their Discord profile avatar.
* **Character Actions**:
  * **Freeze/Unfreeze**: Toggle freezing a player's ped entity to restrict movement.
  * **Noclip**: Trigger a smooth noclip flight mode for spectating or moving quickly.
  * **Godmode**: Grant entity invincibility to protect from damage.
  * **Invisibility**: Render player peds completely invisible.
* **Administrative Enforcement**:
  * **Kick Player**: Disconnect a player with a custom reason.
  * **Ban System**: Ban players temporarily (Hours, Days, Weeks, Months) or Permanently. Saved directly into the database.
* **Teleportation Suite**:
  * **Go To**: Teleport yourself to the target player's exact coordinates.
  * **Bring**: Teleport the target player directly in front of you.
  * **Teleport to Coords**: Set target X, Y, Z, and Heading to teleport anywhere on the map.
* **Inventory Management**: View a target player's inventory items in real-time, delete items, or spawn new items for them.

### 3. Server Controls & Options
* **Time & Weather Control**: Adjust the server's time (via hourly slider) and trigger instant weather changes (e.g., Clear, Rain, Thunder, Fog).
* **Mass Revive Actions**: Revive all dead players server-wide, or dead players within a specified coordinate radius.
* **Clear Area Utility**: Safely clean the server by removing all vehicles, non-player peds, and objects within a set radius.
* **Entity Density Controller**: Slider controls to adjust pedestrian and traffic spawning density dynamically.
* **Coordinate Helper**: Toggle a Text UI overlay displaying current X, Y, Z, and Heading coordinates for easy copying.

### 4. Ticketing & Report System
* **Player Report Command (`/report`)**: Players can submit bug reports or player reports, instantly sending a ticket to all online administrators.
* **Admins Notification Alerts**: Triggers success/error notifications to staff when a new ticket is opened.
* **Ticket Management Panel**: Admins can claim tickets, review target details, and close resolved tickets.
* **Interactive Live Ticket Chat**: Allows admins and the reporter to text back-and-forth in real-time in a dedicated chat thread within the NUI panel (complete with timestamps and Discord avatars).
* **Fast Ticket Actions**: Fast-access buttons inside the ticket detail view to Revive, Go To, Bring, or Kick the reporter/target instantly.

### 5. Dynamic Prop Placement System
* **Interactive Placement Mode**: Spawns a placing prop that admins can move and align before spawning:
  * **W/S Keys**: Adjust X-axis coordinates.
  * **A/D Keys**: Adjust Y-axis coordinates.
  * **Q/Z Keys**: Adjust Z-axis coordinates (height).
  * **Left/Right Arrows**: Rotate prop heading.
* **Confirmation & Sync**: Pressing Enter deletes the placement helper and spawns a permanent mission-entity prop, synced across all nearby clients.
* **Database Persistency**: Spawning a prop with "Save PropData" writes the model, coordinates, heading, and properties to the database, ensuring props reload automatically after server restarts.

### 6. Vehicle Mod Menu
* **Real-time Performance Tuning**: Upgrade engine, brakes, transmission, suspension, turbo, and armor.
* **Cosmetic Tuning system**: Change spoilers, bumpers, side skirts, exhausts, roll cages, grilles, hoods, and fenders.
* **Wheel System Customizer**: Adjust wheel size, wheel width, wheel types (Muscle, Sport, Offroad, High-End), and install custom custom wheels.
* **Visual Styling Options**: Change primary/secondary paint types, neon lighting configurations, custom plate text, and window tint levels.
* **Administrative Vehicle Functions**: Instantly change vehicle ownership database records, update license plate strings, or delete owned vehicles from database tables.

### 7. Collaborative Staff Chat
* **Staff-Only Communication Channel**: A private chat window inside the NUI panel for staff members to align and chat.
* **Detailed Messages**: Shows sender names, timestamps, and pulls Discord avatars for easy identification.
* **Log Persistency**: Holds the last 100 staff messages in server memory for context checks.
