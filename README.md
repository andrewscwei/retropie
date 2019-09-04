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

This process takes a while. While waiting, you can sned a `SIGINFO` signal by pressing `Ctrl+T` to track the progress.

After the image is copied over, you are done. You can insert the SD card to your RaspberryPi and boot up RetroPie for the first time.

## Setup within RetroPie

Run the setup script at `~/RetroPie-Setup/retropie_setup.sh`.

## Connecting to the RetroPie via SFTP

Ensure you are connected to the same network as the RetroPie, then use any method you like to establish an SFTP connection with the following config:

- Protocol: SFTP
- Address: `<address>`
- Port: `22`
- User name: `pi` (default)
- Password: `raspberry` (default)

To obtain the host address of the RetroPie. Either use its external IP or its host name, if available:
  - To get the IP, run `ifconfig` (it's also displayed at the top when you start its shell)
  - To get the host name, run `hostname -f`

## File System Overview

### BIOS

> `~/RetroPie/BIOS`

Each emulator requires respective BIOS files to run. BIOS files are not included by default. Seek official docs to determine what BIOS files are required for the emulators you intend to use.

### ROMs

> `~/RetroPie/roms`

> Official docs: https://github.com/RetroPie/RetroPie-Setup/wiki/Transferring-Roms

Simply add/remove ROMs to manage your games.


### Emulator Configurations

> `opts/retropie/configs`

## Shortcuts

- When you're in Emulation Station, you can exit to shell by pressing `F4` followed by any key. Note that this is assuming default key mapping.
- When you're in shell, you can start Emulation Station via `emulationstation`.
- To shutdown the Pi: `sudo shutdown -h now`
- To reboot the Pi: `sudo reboot`

## Caveats

1. If you are having issues typing `~` in RetroPie shell, try `Shift` + `\`.