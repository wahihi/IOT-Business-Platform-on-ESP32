# 🐳 Docker Guide for ESP32 Development

## 📋 Table of Contents
- [Docker Image Management](#docker-image-management)
- [Container Operations](#container-operations)
- [Container-Host Interaction](#container-host-interaction)
- [ESP32 Project Commands](#esp32-project-commands)
- [Build & Flash Script](#build--flash-script)

## 🖼️ Docker Image Management

### Checking Images
```bash
$ docker images
```
Example output:
```
REPOSITORY      TAG            IMAGE ID       CREATED          SIZE
esp32_start     latest         2af04a5f7a37   12 minutes ago   5.37GB
espressif/idf   release-v5.1   38b56af3b39c   2 days ago      5.21GB
```

### Removing Images
```bash
# Remove by repository and tag
docker rmi espressif/idf:release-v5.1

# Remove by image ID
docker rmi 38b56af3b39c

# Force remove
docker rmi -f espressif/idf:release-v5.1
```

## 🔄 Container Operations

### Running New Containers
```bash
# Basic run with USB device access
docker run -it --device=/dev/ttyACM0 esp32_start

# Mount current directory
docker run -it --device=/dev/ttyACM0 -v $(pwd):/project esp32_start

# Full USB access
docker run -it --privileged esp32_start

# Named container for ESP32 DevKit V1
docker run -it --name ESP32_DevKit_V1 --device=/dev/ttyUSB0 esp32_start

# Named container with specific version
docker run -it --name ESP32_Ver4.4.5 --device=/dev/ttyUSB0 espressif/idf:v4.4.5
```

### Container Management

#### Viewing Containers
```bash
# View running containers
docker ps

# View all containers (including stopped)
docker ps -a

# View latest container
docker ps -l

# Detailed container info
docker inspect [containerID]
```

#### Container Control
```bash
# Start stopped container
docker start [containerID]

# Access running container
docker exec -it [containerID] bash  # with bash
docker exec -it [containerID] sh    # with sh

# Rename container
docker rename old_name new_name
```

### Container Cleanup

```bash
# Remove stopped container
docker rm [containerID]

# Force remove running container
docker rm -f [containerID]

# Remove all stopped containers
docker container prune

# Remove multiple containers
docker rm container1 container2 container3

# Remove all containers (forced)
docker rm -f $(docker ps -aq)

# Remove containers by pattern
docker rm $(docker ps -a | grep "pattern" | awk '{print $1}')
```

## 📂 Container-Host Interaction

### File Copy Operations
```bash
# From host to container (run on host)
docker cp /host/path/file.tar.gz container_name:/container/path

# From container to host (run in container)
docker cp /container/path/file.tar.gz host_name:/host/path
```

## 🔧 ESP32 Project Commands

### Project Setup
```bash
# Create project directory
mkdir wifi_station
cd wifi_station

# Create ESP-IDF project
idf.py create-project simple_wifi
cd simple_wifi
```

### Build Process
```bash
idf.py fullclean
idf.py set-target esp32s3
idf.py menuconfig    # Configure settings
idf.py build
idf.py -p /dev/ttyACM0 flash monitor
```

## 📜 Build & Flash Script

Create `build_flash.sh`:
```bash
#!/bin/bash

echo "🚀 ESP32-S3 Build and Flash Starting..."

echo "1. 🎯 Setting ESP32-S3 target..."
idf.py set-target esp32s3

echo "2. ⚙️ Running menuconfig..."
idf.py menuconfig

echo "3. 🔨 Cleaning and building project..."
idf.py fullclean
idf.py build

echo "4. 📥 Flashing and starting monitor..."
idf.py -p /dev/ttyACM0 flash monitor
```

Make script executable:
```bash
chmod +x build_flash.sh
```

Run script:
```bash
./build_flash.sh
```

> 💡 **Tip**: To exit container without stopping (detach), press `Ctrl + P, Ctrl + Q` in sequence.

## 🎨 Style Guide
- 🔵 Commands are shown in code blocks
- 🟡 Important notes are highlighted
- 🟢 Tips and tricks are marked with emoji
- 🔴 Warnings and cautions are clearly marked
