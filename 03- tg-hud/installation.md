# Installation

Follow these steps to install TG HUD on your FiveM server.

## 1. Download & unzip

Download the TG HUD package and extract it.

## 2. Add the resource to your server

Move the `tg-hud` folder into your server's resources directory.

## 3. Install dependencies

Make sure the required dependencies are installed:

- `ox_lib`
- `tg-core` (if required by your configuration)

## 4. Update your server.cfg

Add the following line after starting the dependencies:

```cfg
ensure ox_lib
ensure tg-core
ensure tg-hud
```

## 5. Configure the script

Open `config/config.lua` and adjust the available settings to your server's requirements.

Your TG HUD is now ready to use!