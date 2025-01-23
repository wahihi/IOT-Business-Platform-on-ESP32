```markdown
# Quick Start Guide for ESP32 CI/CD Pipeline Setup

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Docker Setup](#docker-setup)
- [GitHub Actions Setup](#github-actions-setup)
- [Slack Integration](#slack-integration)
- [Complete Workflow Setup](#complete-workflow-setup)
- [Troubleshooting Guide](#troubleshooting-guide)

## Overview
This guide explains how to quickly set up a CI/CD pipeline for ESP32 development, integrating Docker, GitHub Actions, and Slack for automated build and test environments.

## Prerequisites
- Docker installed
- GitHub account
- Slack workspace
- ESP32 development board
- USB-Serial driver

## Docker Setup

1. Create ESP32 Docker image
```dockerfile
FROM espressif/idf:release-v5.1

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    git \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /root/esp
```

2. Run Docker container
```bash
docker build -t esp32_cicd .
docker run -it --device=/dev/ttyUSB0 esp32_cicd
```

## GitHub Actions Setup

1. Repository Configuration
- Add self-hosted runner in Settings > Actions > Runners
- Configure Workflow permissions in Settings > Actions > General
  - Select "Read and write permissions"
  - Check "Allow GitHub Actions to create and approve pull requests"

2. Create Workflow File (.github/workflows/esp32_cicd.yml)
```yaml
name: ESP32 Complete Automation
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  workflow_dispatch:

permissions:
  issues: write
  contents: read

env:
  ESP_DEVICE_PORT: "/dev/ttyUSB0"
  ESP_MONITOR_PORT: "/dev/ttyUSB0"
  FIRMWARE_PATH: "examples/CI-CD-Pipeline-TEST/hello_world/build/hello_world.bin"

jobs:
  build:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v3
      # ... build steps ...

  deploy-and-test:
    needs: build
    runs-on: self-hosted
    steps:
      # ... deploy and test steps ...
```

## Slack Integration

1. Slack App Setup
- Add 'Incoming Webhooks' app to Slack workspace
- Select notification channel
- Copy Webhook URL

2. GitHub Secrets Setup
- Navigate to Settings > Secrets and variables > Actions
- Click 'New repository secret'
- Name: `SLACK_WEBHOOK_URL`
- Secret: Paste Webhook URL

## Complete Workflow Setup
```yaml
[Complete workflow YAML content]
```

## Troubleshooting Guide

### Common Issues

1. Serial Port Access Permissions
```bash
sudo chmod 666 /dev/ttyUSB0
```

2. ESP32 Monitoring Errors
- For TTY errors:
```yaml
- name: Monitor and capture logs
  run: |
    timeout 120s idf.py monitor -p ${ESP_MONITOR_PORT} > esp32_logs.txt 2>&1 || true
```

3. GitHub Issue Creation Permission Error
- Verify Workflow permissions
- Add `permissions` section:
```yaml
permissions:
  issues: write
  contents: read
```

4. Slack Notification Failures
- Verify Webhook URL
- Check Secrets configuration

### Debugging Tips
1. Enable Debug Logging
```yaml
- name: Debug info
  run: |
    echo "Serial port status:" > debug_logs.txt
    ls -l /dev/ttyUSB0 >> debug_logs.txt
```

2. Check Failed Job Artifacts
```yaml
- name: Upload debug logs
  if: always()
  uses: actions/upload-artifact@v3
  with:
    name: debug-logs
    path: debug_logs.txt
```

## License
MIT License

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.
```
