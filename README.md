# Tetra-TMO-Base-Stations
Tetra TMO base stations for SDR Transceivers

TMO BS from BlueStation
# https://github.com/MidnightBlueLabs/tetra-bluestation

TMO BS from FlowStation
# https://github.com/razvanzeces/flowstation

TMO BS from Nexus
# https://github.com/invictus737/nexus-bs

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

4. RPi-setup:
"enable SPI and I2C"
# sudo raspi-config > Interface Options > "enable and reboot the rpi"
# sudo nano /boot/firmware/config.txt
- "add this param if the eeprom cannot be recognized"
# dtparam=i2c_vc=on
- Ctrl + O and Ctrl + X
- Reboot the Rpi "sudo reboot now"

5. Install SoapySX:
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

6. Install The FlowStation:
# cd
# git clone https://github.com/razvanzeces/flowstation
# cd flowstation
# . "$HOME/.cargo/env"
# cargo build --release (or "cargo build --release -j1" for Rpi with less then 2GB of RAM or Rpi3)
- To install with "Asterisk"
# cargo build --release --features asterisk
# cp example_config/config.toml config.toml
# nano config.toml "configure the base station"
- Ctrl + O and Ctrl + X
- After configured "config.toml" make a fallback copy
# cp config.toml config.toml.fallback

- In case the compiling cannot be done with RPi/1GB then extend the Zram partition from 905MB to 2048MB:
- cd
- sudo nano /etc/rpi/swap.conf
- [Zram]
- FixedSizeMiB=2048 (uncheck)
- save and close
- sudo reboot now
- zramctl

7. Run:
# cd flowstation
# ./target/release/bluestation-bs ./config.toml

8. Update: Standard
# git pull
# cargo build --release 

9. Run Autostart
# cd flowstation
# sudo cp contrib/systemd/bluestation-bs.service /etc/systemd/system/tetra.service
# cd
# sudo nano /etc/systemd/system/tetra.service
- edit the "tetra.service" (in my case)

- [Service]
- User=fs
- Group=fs
- Type=simple
- CPUSchedulingPolicy=fifo
- CPUSchedulingPriority=73
- WorkingDirectory=/home/fs/flowstation

  # # Reset the SXceiver/SoapySDR USB device before each start to prevent
  # # stale hardware timestamp state causing "Too late to produce TX block" skips
  # # on software restarts (as opposed to hard power cycles).
  # # This unbinds and rebinds the USB device, forcing a clean hardware reset.
  # # Adjust the USB path to match your device: find it with:
  # #   udevadm info -q path -n /dev/bus/usb/$(lsusb | grep -i sxceiver | awk '{print $2"/"$4}' | tr -d ':')
  # # or simply: ls /sys/bus/usb/devices/
  # # ExecStartPre=/bin/sh -c 'for dev in /sys/bus/usb/devices/*/idVendor; do dir=$(dirname $dev); [ -f "$dir/authorized" ] && echo 0 > "$dir/authorized" && sleep 0.5 && echo 1 > "$dir/authorized"; done; sleep 1'

- ExecStart=/home/fs/flowstation/target/release/bluestation-bs /home/fs/flowstation/config.toml
- KillSignal=SIGINT

# systemctl daemon-reload
# systemctl enable --now tetra
- systemctl start --now tetra
- systemctl stop --now tetra
- systemctl status --now tetra

10. Update for autorun
# systemctl stop --now tetra
# cd flowstation
# git pull
# cargo build --release
# cd
# systemctl start --now tetra
- or -
# sudo reboot now
