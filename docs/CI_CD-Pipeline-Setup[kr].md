```markdown
# ESP32 개발을 위한 CI/CD 파이프라인 Quick Start 가이드

## 목차
- [개요](#개요)
- [사전 준비사항](#사전-준비사항)
- [도커 환경 설정](#도커-환경-설정)
- [GitHub Actions 설정](#github-actions-설정)
- [Slack 연동](#slack-연동)
- [전체 워크플로우 설정](#전체-워크플로우-설정)
- [문제해결 가이드](#문제해결-가이드)

## 개요
이 가이드는 ESP32 개발을 위한 CI/CD 파이프라인을 빠르게 구축하는 방법을 설명합니다. Docker, GitHub Actions, Slack을 통합하여 자동화된 빌드 및 테스트 환경을 구성합니다.

## 사전 준비사항
- Docker 설치
- GitHub 계정
- Slack 워크스페이스
- ESP32 개발 보드
- USB-Serial 드라이버

## 도커 환경 설정

1. ESP32 도커 이미지 생성
```dockerfile
FROM espressif/idf:release-v5.1

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    git \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /root/esp
```

2. 도커 컨테이너 실행
```bash
docker build -t esp32_cicd .
docker run -it --device=/dev/ttyUSB0 esp32_cicd
```

## GitHub Actions 설정

1. 레포지토리 설정
- Settings > Actions > Runners에서 self-hosted runner 추가
- Settings > Actions > General에서 Workflow permissions 설정
  - "Read and write permissions" 선택
  - "Allow GitHub Actions to create and approve pull requests" 체크

2. Workflow 파일 생성 (.github/workflows/esp32_cicd.yml)
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
      # ... 빌드 스텝 ...

  deploy-and-test:
    needs: build
    runs-on: self-hosted
    steps:
      # ... 배포 및 테스트 스텝 ...
```

## Slack 연동

1. Slack App 설정
- Slack 워크스페이스에서 'Incoming Webhooks' 앱 추가
- 알림을 받을 채널 선택
- Webhook URL 복사

2. GitHub Secrets 설정
- Settings > Secrets and variables > Actions
- 'New repository secret' 클릭
- Name: `SLACK_WEBHOOK_URL`
- Secret: Webhook URL 붙여넣기

## 전체 워크플로우 설정
```yaml
[전체 워크플로우 YAML 파일 내용]
```

## 문제해결 가이드

### 일반적인 문제들

1. Serial 포트 접근 권한
```bash
sudo chmod 666 /dev/ttyUSB0
```

2. ESP32 모니터링 오류
- TTY 오류 발생 시:
```yaml
- name: Monitor and capture logs
  run: |
    timeout 120s idf.py monitor -p ${ESP_MONITOR_PORT} > esp32_logs.txt 2>&1 || true
```

3. GitHub Issue 생성 권한 오류
- Workflow permissions 확인
- `permissions` 섹션 추가:
```yaml
permissions:
  issues: write
  contents: read
```

4. Slack 알림 실패
- Webhook URL 확인
- Secrets 설정 확인

### 유용한 디버깅 팁
1. Debug 로그 활성화
```yaml
- name: Debug info
  run: |
    echo "Serial port status:" > debug_logs.txt
    ls -l /dev/ttyUSB0 >> debug_logs.txt
```

2. 실패한 작업의 아티팩트 확인
```yaml
- name: Upload debug logs
  if: always()
  uses: actions/upload-artifact@v3
  with:
    name: debug-logs
    path: debug_logs.txt
```

## 라이선스
MIT License

## 기여하기
Pull requests는 환영합니다. 주요 변경사항의 경우, 먼저 issue를 열어 논의해주세요.
```
