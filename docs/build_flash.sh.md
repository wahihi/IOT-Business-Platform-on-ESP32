#!/bin/bash

echo "ESP32-S3 build & flash start.."
# . /opt/esp/idf/export.sh

echo "1. ESP32-S3 target set..."
idf.py set-target esp32

# echo "2. menuconfig ..."
# idf.py menuconfig

echo "3. project clean & build..."
#idf.py fullclean
idf.py build

echo "4. flash download & monitor..."
idf.py -p /dev/ttyACM0 flash monitor
