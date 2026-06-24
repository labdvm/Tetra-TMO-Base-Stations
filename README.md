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

7. Run:
# cd flowstation
# ./target/release/bluestation-bs ./config.toml

8. Update: Standard
# git pull
# cargo build --release 
