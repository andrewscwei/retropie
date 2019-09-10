# RetroPie

> Official docs: https://github.com/RetroPie/RetroPie-Setup/wiki

## Setup

### Flashing the RetroPie Distribution

This process involves flashing the RetroPie distribution to the SD card you intend to use for your RaspberryPi. First, download the distro [here](https://retropie.org.uk/download/) for your specific RaspberryPi model, then unzip the downloaded file.

Follow this guide for instructions on how to flash an SD card: https://www.raspberrypi.org/documentation/installation/installing-images/. Alternatively you can read on for a condensed set of steps for macOS.

First, plug in your SD card on your Mac and identify the BSD name, i.e. `/dev/disk2`:

```sh
$ diskutil list
```

Unmount the disk:

```sh
$ diskutil unmountDisk /dev/disk<disk# from diskutil>
```

Copy the downloaded image to the disk:

```sh
$ sudo dd bs=1m if=<path to .img> of=/dev/rdisk<disk# from diskutil> conv=sync
```

This process takes a while. While waiting, you can send a `SIGINFO` signal by pressing `Ctrl+T` to track the progress.

After the image is copied over, you are done. You can insert the SD card to your RaspberryPi and boot up RetroPie for the first time.

### First Boot

Upon booting up the RetroPie for the first time, it will run a few scripts to take care of the initial configurations. This may take a few minutes. On complete, the Pi will reboot itself. Do not manually restart the Pi if you see a lingering black screen, the Pi is just rebooting itself.

Emulation Station will then be launched, and you will need to set up your first controller. A keyboard is recommended. Simply follow on-screen instructions.

### Switching Between Shell and Emulation Station

- To go to Shell from Emulation Station: `F4`
- To go to Emulation Station from Shell: `emulationstation`

### Configure Wi-Fi

1. `sudo raspi-config`
2. **Network Options** -> **Wi-Fi**

### Enabling SSH and SFTP

1. `sudo raspi-config`
2. **Interfacing Options** -> **SSH** -> enable

### Configuring the Display

If you see that the display is not fullscreen:
1. `sudo nano /boot/config.txt`
2. Uncomment `# disable_overscan=1`

### Changing the splash screen

1. SSH/SFTP into the Pi and copy your splash image to `/home/pi/RetroPie/splashscreens`
2. From Emulation Station menu, go to RetroPie -> **Splash Screens** -> **Choose splashscreen** -> **Own/Extra splashscreens**

## BIOS

> Location: `~/RetroPie/BIOS`

Some emulators require designated BIOS files to run. BIOS files are not included by default. Seek official docs to determine what BIOS files are required for the emulators you intend to use.

## ROMs

> Location: `~/RetroPie/roms`

> Official docs: https://github.com/RetroPie/RetroPie-Setup/wiki/Transferring-Roms

Simply add/remove ROMs to manage your games.

## Emulator Configurations

> Location: `opts/retropie/configs`

Edit `retroarch.cfg` of respective emulators.

## Updating

Run the setup script at `~/RetroPie-Setup/retropie_setup.sh`. First choose to update the setup script, then select **Update** to update all packages.

## Themes

### Browsing the Theme Gallery

1. From ES, go to RetroPie -> **ES Themes**
2. Select **Download Theme Gallery**
3. When done, a new option will open up: **View or Update Theme Gallery** -> **View Theme Gallery**

### Installing a Theme

1. From ES, go to RetroPie -> **ES Themes**
2. Select a theme to install
3. Return to ES, toggle Start menu -> **UI Settings** -> pick from **Theme Set**

## Connecting to the RetroPie via SFTP

Ensure you are connected to the same network as the RetroPie, then use any method you like to establish an SFTP connection with the following config:

- Protocol: SFTP
- Address: `<address>`
- Port: `22`
- User name: `pi` (default)
- Password: `raspberry` (default)

To obtain the host address of the RetroPie. Either use its external IP or its host name, if available:
  - To get the IP, run `ifconfig` (it's also displayed at the top when you start its Shell)
  - To get the host name, run `hostname -f`

## Connecting to the RetroPie via SSH

### Via Wi-Fi

Just make sure you are on the same network as the Pi, then connect to it using either one of the following methods:
1. By IP: `ssh pi@<ip_address>` (you can scan for all devices connected to your network via `arp -a`)
2. By host name: `ssh pi@<host_name>` (you can get the host name of the Pi by `hostname -f` inside the Pi's Shell)

### Via Ethernet Cable

Assuming you are on a Mac, first, enable network sharing on the ethernet cable:
1. Go to **System Preferences** -> **Sharing**
2. Select **Internet Sharing** on the left (just select, don't check, you probably can't yet)
3. On the right, leave **Share your connection from:** as `Wi-Fi`, then select the port corresponding to your Ethernet cable (i.e. `USB 10/100/1000 LAN`, hint: you can find out the name of the port by first connecting your Ethernet cable to your Mac then go to **System Preferences** -> **Network** and see which one is newly enabled)
4. Check the checkbox for **Internet Sharing**
5. Go to **System Preferences** -> **Network** and see that there is a newly enabled network, ensure that the **Configure IPv4** option is **Using DHCP**
6. From Terminal, run `ifconfig` and look for your IP address (aliased `inet`) for the `bridge100` option (i.e. `192.168.2.1`)
7. Scan for connected devices with the above IP address by running `nmap -n -sP <ip_address>/24`
8. Note that there should be multiple scanned results, with the first one being your own device (i.e. `192.168.2.1`), and the other one (or one of the other ones) being the Pi
9. Simply connect to it via `ssh pi@<ip_address>`

## Pairing PS4 Controller via Bluetooth

1. `sudo ~/RetroPie-Setup/retropie_setup.sh`
2. **Configuration / tools** -> **804 bluetooth** -> **Register and Connect to Bluetooth Device**
3. Put PS4 controller in pairing mode by holding **Share** button first followed by **PS** button until the LED flashes
4. When paired, choose the default (`DisplayYesNo`) as security mode

## Key Mapping

> Official docs: https://github.com/RetroPie/RetroPie-Setup/wiki/RetroArch-Configuration#hardcoded-configurations

You can map your keys from ES by pressing the Start menu and selecting **Configure Input**, then following the on-screen instructions.

To customize keys manually through the config files, note the following behavior. RetroArch has a global key mapping file stored at `/opt/retropie/configs/all/retroarch.cfg`. This file is used by all emulators. You can override the settings defined in this file per emulator by modifying `/opt/retropie/configs/<emulator>/retroarch.cfg`.

For wireless controllers such as the PS4 controller, things are a bit different. The global key mapping config files for wireless controllers are stored in `/opt/retropie/configs/all/retroarch-joypads`, which symlinks to `/opt/retropie/configs/all/retroarch/autoconfig/`. To override wireless controller mappings for specific emulators, do the following:

1. Create a new directory called `retroarch-joypads` in `/opt/retropie/configs/<emulator>`.
2. Edit `/opt/retropie/configs/<emulator>/retroarch.cfg` and add this line `joypad_autoconfig_dir = "/opt/retropie/configs/<emulator>/retroarch-joypads/"` before the `#import` line.
3. Copy `/opt/retropie/configs/all/retroarch-joypads/Wireless Controller.cfg` (or equivalent) to `/opt/retropie/configs/<emulator>/retroarch-joypads`.
4. Override your key mappings in `/opt/retropie/configs/<emulator>/retroarch-joypads/Wireless Controller.cfg` (or equivalent).

## Hiding Certain Emulators

Emulators appear in ES as long as they have ROMs in them. Simply remove the ROM files from `~/RetroPie/roms/<emulator>/` and they will disappear from ES. You can also just create a hidden folder in `~/RetroPie/roms` (i.e. `~/RetroPie/roms/.unused`) and move the emulator folder there.

## Support Multiple Languages

Install the following unicode fallback font:

```sh
$ sudo apt-get install fonts-droid-fallback
```

Chances are it just works for your theme, but if not you might find some success modifying `/etc/emulationstation/themes/<theme_name>/theme.xml`.

## Changing Game Titles

Copy `/opt/retropie/configs/all/emulationstation/gamelists/<emulator>/gamelist.xml` to `~/RetroPie/roms/<emulator>/gamelist.xml`, and edit it there.

## Better Scraping

> Official docs: https://github.com/RetroPie/RetroPie-Setup/wiki/scraper

1. `sudo ~/RetroPie-Setup/retropie_setup.sh`
2. **Manage packages** -> **Manage optional packages**
3. Look for **scraper** in the list and install

## In-game Shortcuts

- To acess the RGUI on keyboard: `Hotkey + F1`
- To acess the RGUI on controller: `Hotkey + X`

## Common Commands

- When you're in Emulation Station, you can exit to Shell by pressing `F4` followed by any key. Note that this is assuming default key mapping.
- When you're in Shell, you can start Emulation Station via `emulationstation`.
- To shutdown the Pi: `sudo shutdown -h now`
- To reboot the Pi: `sudo reboot`

## Caveats

1. If you are having issues with keyboard characters, i.e. typing `~` becomes something else, configure the keyboard layout:
    1. `sudo raspi-config`
    2. **Localisation Options** -> **Change Keyboard Layout** -> **Generic 105-key (Intl) PC** -> **English (US)**
2. If you are having issues with lagged/crackling audio on SNES, switch the emulator to `lr-snes9x2002`.
