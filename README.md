# Tetra-TMO-Base-Stations
- Tetra TMO base stations for SDR Transceivers

TMO BS from BlueStation (limited options)
# https://github.com/MidnightBlueLabs/tetra-bluestation

TMO BS from FlowStation
# https://github.com/razvanzeces/flowstation

TMO BS from Bust-FlowStation (advanced flowstation)
# https://github.com/Aitorrio/bost-flowstation

TMO BS from YO9ION (modified Flowstation)
# https://github.com/YO9ION/fs2-kit

Multi-Mode TetraSpot (Tetra-TMO, DMR TierII-III, P25 Phase-1, NXDN, D-Star, M17, FM-Analog)
# https://update.tetra-poland.pl/

TMO BS from Nexus
# https://github.com/invictus737/nexus-bs (archived V1-tetra only)
# https://github.com/invictus737/nexus-bs2 (Multi-Mode) ...soon ......maybe next year !?

# Build:
- Install FlowStation ---for sxceiver---
# 1. Install Trixie 64bit-LITE
- sudo apt update && sudo apt full-upgrade
2. Install lib's:
# sudo apt install git make g++ cmake
# sudo apt install libsoapysdr-dev
# sudo apt install soapysdr-tools
# sudo apt install libasound2-dev
# sudo apt install clang llvm-dev libclang-dev

3. Install Rust:
# curl https://sh.rustup.rs -sSf | sh

4. Install SoapySX:
# cd
# git clone "https://github.com/tejeez/sxxcvr.git"
# cd sxxcvr/SoapySX
# mkdir build
# cd build
# cmake ..
# make
# sudo make install
# sudo ldconfig

Check that the driver is available:
# SoapySDRUtil --info

Check that the device is detected:
# ls -l /proc/device-tree/hat
# SoapySDRUtil --probe=driver=sx

5. Install The FlowStation:
# cd
# git clone https://github.com/razvanzeces/flowstation
# cd flowstation
# . "$HOME/.cargo/env"
# cargo build --release (or "cargo build --release -j1" for Rpi with less then 2GB of RAM or Rpi3)
# cp example_config/config.toml config.toml
# nano config.toml (configure the base station) (https://bluestation.russel053.com/)
- In case the compiling cannot be done with RPi3/1GB then extend the Zram partition from 905MB to 2048MB:
- cd
- sudo nano /etc/rpi/swap.conf
- [Zram]
- FixedSizeMiB=2048 (uncheck)
- save and close
- sudo reboot now
- zramctl
  

6. Run:
# cd flowstation
# ./target/release/bluestation-bs ./config.toml

7. Update: Standard
# git pull
# cargo build --release 

8. Run Autostart
# cd flowstation
# sudo cp contrib/systemd/bluestation-bs.service /etc/systemd/system/tetra.service
# cd
# sudo nano /etc/systemd/system/tetra.service
- edit the "tetra.service" (in my case)

- [Service]
- User=fs (change it)
- Group=fs (change it)
- Type=simple
- CPUSchedulingPolicy=fifo
- CPUSchedulingPriority=73
- WorkingDirectory=/home/fs/flowstation (/home/"change it"/flowstation

  # # Reset the SXceiver/SoapySDR USB device before each start to prevent
  # # stale hardware timestamp state causing "Too late to produce TX block" skips
  # # on software restarts (as opposed to hard power cycles).
  # # This unbinds and rebinds the USB device, forcing a clean hardware reset.
  # # Adjust the USB path to match your device: find it with:
  # #   udevadm info -q path -n /dev/bus/usb/$(lsusb | grep -i sxceiver | awk '{print $2"/"$4}' | tr -d ':')
  # # or simply: ls /sys/bus/usb/devices/
  # # ExecStartPre=/bin/sh -c 'for dev in /sys/bus/usb/devices/*/idVendor; do dir=$(dirname $dev); [ -f "$dir/authorized" ] && echo 0 > "$dir/authorized" && sleep 0.5 && echo 1 > "$dir/authorized"; done; sleep 1'

- ExecStart=/home/fs/flowstation/target/release/bluestation-bs /home/fs/flowstation/config.toml (/home/"change it"/flowstation/target/release/bluestation-bs /home/"change it"/flowstation/config.toml
- KillSignal=SIGINT

# systemctl daemon-reload
# systemctl enable --now tetra
- systemctl start --now tetra
- systemctl stop --now tetra
- systemctl status --now tetra

9. Update for autorun
# systemctl stop --now tetra
# cd flowstation
# git pull
# cargo build --release
# cd
# systemctl start --now tetra
- or -
# sudo reboot now
