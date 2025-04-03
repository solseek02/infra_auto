# PC2 인프라 자동화 프로젝트

## 📋 프로젝트 개요
이 프로젝트는 Ansible을 사용하여 PC2 인프라를 자동화하는 솔루션입니다. 방화벽, DB 방화벽, DB 서버, 백업, SIEM, 로그 전송 등의 구성 요소를 자동으로 설정하고 관리합니다.

## 🚀 주요 기능
- 방화벽 자동 구성
- DB 방화벽 자동 구성
- DB 서버 (Master-Slave) 자동 구성
- 백업 시스템 자동 구성
- SIEM 시스템 자동 구성
- 로그 전송 시스템 자동 구성

## 🛠 기술 스택
- Ansible
- Firewalld
- MariaDB
- Keepalived
- Elasticsearch
- Kibana
- Filebeat
- Metricbeat

## 📁 프로젝트 구조
```
pc2/
├── firewall/          # 방화벽 설정
│   ├── inventory.ini
│   └── playbook.yml
├── db_firewall/       # 데이터베이스 방화벽 설정
│   ├── inventory.ini
│   └── playbook.yml
├── db_server/         # 데이터베이스 서버 설정
│   ├── inventory.ini
│   └── playbook.yml
├── backup/            # 백업 설정
│   ├── inventory.ini
│   └── playbook.yml
├── siem/             # SIEM 설정
│   ├── inventory.ini
│   ├── playbook.yml
│   └── file/         # SIEM 관련 파일
│       ├── elasticsearch.repo
│       ├── elasticsearch.yml
│       └── kibana.yml
└── log_transfer/     # 로그 전송 설정
    ├── inventory.ini
    └── playbook.yml
```

## 🔧 시스템 요구사항

### 필수 소프트웨어
- Ansible 2.12 이상
- Python 3.9 이상
- SSH 접근 권한

### 하드웨어 요구사항
- **방화벽**
  - CPU: 최소 2코어
  - RAM: 최소 4GB
  - Storage: 최소 20GB

- **DB 방화벽**
  - CPU: 최소 2코어
  - RAM: 최소 4GB
  - Storage: 최소 20GB

- **DB 서버**
  - CPU: 최소 4코어
  - RAM: 최소 8GB
  - Storage: 최소 100GB

- **백업 서버**
  - CPU: 최소 2코어
  - RAM: 최소 4GB
  - Storage: 최소 100GB

- **SIEM 서버**
  - CPU: 최소 4코어
  - RAM: 최소 8GB
  - Storage: 최소 50GB

- **로그 전송 서버**
  - CPU: 최소 2코어
  - RAM: 최소 4GB
  - Storage: 최소 20GB

## ⚙️ 설치 및 설정

### 1. 사전 준비
```bash
# 시스템 업데이트
sudo dnf update -y

# EPEL 저장소 설치
sudo dnf install epel-release -y

# Ansible 설치
sudo dnf install ansible-core -y

# 필요한 모듈 설치
ansible-galaxy collection install community.mysql
ansible-galaxy collection install ansible.posix
```

### 2. SSH 키 설정
```bash
# SSH 키 생성
ssh-keygen -t rsa -b 4096

# 공개키 복사
ssh-copy-id root@172.16.6.82
```

### 3. Ansible 플레이북 실행
```bash
# 방화벽 구성
ansible-playbook -i firewall/inventory.ini firewall/playbook.yml

# DB 방화벽 구성
ansible-playbook -i db_firewall/inventory.ini db_firewall/playbook.yml

# DB 서버 구성
ansible-playbook -i db_server/inventory.ini db_server/playbook.yml

# 백업 구성
ansible-playbook -i backup/inventory.ini backup/playbook.yml

# SIEM 구성
ansible-playbook -i siem/inventory.ini siem/playbook.yml

# 로그 전송 구성
ansible-playbook -i log_transfer/inventory.ini log_transfer/playbook.yml
```

## 🔍 상세 구성

### 방화벽
- Firewalld 설치 및 설정
- 포트 포워딩 설정
- NAT 마스커레이딩 설정
- ICMP 및 TCP/UDP 포트 설정

### DB 방화벽
- Firewalld 설치 및 설정
- DB 포트 포워딩 설정
- NAT 마스커레이딩 설정
- 보안 정책 적용

### DB 서버
- MariaDB Master-Slave 구성
- Keepalived 설정
- 복제 사용자 설정
- 보안 설정

### 백업 시스템
- Restic 설치 및 설정
- 백업 저장소 초기화
- 백업 스케줄 설정

### SIEM 시스템
- Elasticsearch 설치 및 설정
- Kibana 설치 및 설정
- 인증 설정
- 대시보드 설정

### 로그 전송 시스템
- Filebeat 설치 및 설정
- Metricbeat 설치 및 설정
- 시스템 모듈 설정
- 로그 전송 설정

## ⚠️ 주의사항
1. **실행 전 확인사항**
   - 모든 서버에 대한 SSH 접근이 가능한지 확인
   - 충분한 디스크 공간이 있는지 확인
   - 네트워크 연결이 안정적인지 확인

2. **중요 설정**
   - 방화벽 규칙은 신중하게 설정
   - DB 복제 설정은 정확히 확인
   - 백업은 정기적으로 검증

3. **문제 해결**
   - 로그 파일 확인: `/var/log/messages`
   - 서비스 상태 확인: `systemctl status [서비스명]`
   - 방화벽 규칙 확인: `firewall-cmd --list-all`

## 📝 변경 이력
- 2024-04-03: 초기 릴리스
  - 방화벽 구성
  - DB 방화벽 구성
  - DB 서버 구성
  - 백업 시스템 구성
  - SIEM 시스템 구성
  - 로그 전송 시스템 구성

## 📜 라이센스
이 프로젝트는 MIT 라이센스 하에 배포됩니다.

## 👥 기여
버그 리포트나 기능 제안은 Issue를 통해 해주세요.
Pull Request는 항상 환영합니다.

## 📞 문의
문제가 있거나 도움이 필요하시면 다음으로 연락주세요:
- 이메일: [solseek02@gmail.com]
- GitHub Issues: [https://github.com/solseek02/infra_auto.git]/issues 