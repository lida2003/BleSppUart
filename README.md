# BleSppUart

BleSppUart is try to support basic function (BetaFlight configurator) of SpeedyBee App.

# Solution

## Hardware

| Supported Targets | ESP32 | ESP32-C2 | ESP32-C3 | ESP32-H2 | ESP32-S3 |
| :---------------: | :---: | :------: | :------: | :------: | :------: |
| TBD | Tested | TBD | TBD | TBD | TBD |

## Software 

ESP-IDF GATT SERVER SPP Example is based.
>[esp-idf-v5.0](https://github.com/espressif/esp-idf/tree/release/v5.0) \examples\bluetooth\bluedroid\ble\ [ble_spp_server](https://github.com/espressif/esp-idf/tree/release/v5.0/examples/bluetooth/bluedroid/ble/ble_spp_server)

# How to Dev & Test

## Step 1: create a working dir

> $ mkdir test
> 
> $ cd test

## Step 2: clone ESP IDF v5.0

> $ git clone git@github.com:espressif/esp-idf.git
> 
> $ cd esp-idf
> 
> $ git checkout release/v5.0

## Step 3: setup dev environment

> $ ./install.sh

## Step 4: clone BleSppUart

> $ cd ..
> 
> $ git clone git@github.com:lida2003/BleSppUart.git

## Step 5: enter dev environment

> $ cd esp-idf
> 
> $ . ./export.sh

## Step 6: build BleSppUart

> $ cd BleSppUart
> 
> $ idf.py set-target ESP32
> 
> $ idf.py build

*Note: try idf.py set-target target, if you have other module or dev boards.*

## Step 7: update firmware

> $ idf.py -p /dev/ttyUSB0 flash monitor

*Note: change ttyUSBx, x=serial port of the ttl module.*

# Summary

Current test result shows BLE-SPP UART is NOT supporting SpeedyBee. 

So my premire rootcause is SpeedyBee has it's own protocol between mobile app and BLE SPP device through self defined truncate package format, etc.

Hope SpeedyBee can share more info.

# Suspection

## BLE SPP demo, default MTU size is 23
> Wich suggests using multi packet transmission of communication frame.
> Since I didn't see any other value negotiated. So I think 23 is used for SpeedyBee.

## Truncate package format
> **Full MTU package**
> 
> '#' + '#' + total_num + current_num + rx_buffer[(spp_mtu_size-7)]
> 
> **Last package**
> 
> '#' + '#' + total_num + current_num + rx_buffer[(event.size - (current_num - 1)*(spp_mtu_size - 7))]
> 
> There are multi-times request for long packet response. it seems ok with UUI, API version and FC version msp commands.
> [Not Compatible with SpeedyBee App #1](https://github.com/lida2003/BleSppUart/issues/1)
