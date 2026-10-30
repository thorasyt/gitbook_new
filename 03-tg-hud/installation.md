# Installation

Follow these steps to install TG HUD on your FiveM server.

## 1. Download & unzip

Download the TG HUD package and extract it.

## 2. Add the resource to your server

Move the `tg-hud` folder into your server's resources directory.

## 3. Install dependencies

Make sure the required dependencies are installed:

- `ox_lib`

## 4. Update your server.cfg

Add the following line after starting the dependencies:

```cfg
ensure ox_lib
ensure es_extended or qbx_core
ensure fuel_script
ensure tg-hud
```

## 5. Configure the script

Open `shared` folder and adjust the available settings to your server's requirements.

Your TG HUD is now ready to use!