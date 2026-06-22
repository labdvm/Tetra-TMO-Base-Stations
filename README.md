# Tetra-TMO-Base-Stations
Tetra TMO base stations for SDR Transceivers

TMO BS from BlueStation
# https://github.com/MidnightBlueLabs/tetra-bluestation

TMO BS from FlowStation
# https://github.com/razvanzeces/flowstation

TMO BS from Nexus
# https://github.com/invictus737/nexus-bs

- Build:

- Install FlowStation ---for sxceiver---
# 1. Install Trixie 64bit-LITE
2. Install lib's:
# sudo apt update
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
"add"
# dtparam=i2c_vc=on
"Reboot the Rpi"

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
# git clone https://github.com/razvanzeces/flowstation
# cd flowstation
# . "$HOME/.cargo/env"
# cargo build --release (or cargo build --release -j1)
# cp example_config/config.toml config.toml
# nano config.toml "configure the base station"
- After configured "config.toml" make a fallback copy
# cp config.toml config.toml.fallback

7. Run:
# cd flowstation
# ./target/release/bluestation-bs ./config.toml

8. Update:
# git pull
# cargo build --release
