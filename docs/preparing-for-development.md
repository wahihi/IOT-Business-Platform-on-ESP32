# 1. Preparing for Development

## Prerequisites
- Docker installed on your system
  - [Detailed docker commands](./Docker-Environment-Setup&ESP32-Build-Guide.md)
- ESP32 development board
  - [Simple idf build commands](./build_flash.sh.md)  
- USB cable for firmware flashing
- Basic understanding of IOT concepts

## Environment Setup
1. Pull the Docker image:
   ```bash
   docker pull espressif/idf:release-v5.1
   ```

2. Create and run a container:
   ```bash
   docker run -it --name ESP32_DevKit_V1 --device=/dev/ttyUSB0 espressif/idf:release-v5.1
   ```

3. Verify the setup:
   ```bash
   idf.py --version
   ```
