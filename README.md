# 인프라 자동화 프로젝트 (PC1 & PC2)

## 📋 프로젝트 개요
이 프로젝트는 Ansible을 사용하여 PC1과 PC2의 인프라스트럭처를 자동으로 구성하고 관리하는 자동화 솔루션입니다. PC1은 DNS 서버, 메일 서버, 쿠버네티스 클러스터를 포함하고, PC2는 방화벽, DB 방화벽, DB 서버, 백업, SIEM, 로그 전송 등의 구성 요소를 포함합니다.

## 🚀 주요 기능

### PC1 주요 기능
- DNS 서버 자동 구성
- 메일 서버 자동 구성
- 쿠버네티스 클러스터 자동 구성
- 시스템 보안 설정 자동화
- 모니터링 설정 자동화

### PC2 주요 기능
- 방화벽 자동 구성
- DB 방화벽 자동 구성
- DB 서버 (Master-Slave) 자동 구성
- 백업 시스템 자동 구성
- SIEM 시스템 자동 구성
- 로그 전송 시스템 자동 구성

## 🛠 기술 스택

### PC1 기술 스택
- **Ansible**: 인프라 자동화
- **BIND**: DNS 서버
- **Postfix & Dovecot**: 메일 서버
- **Kubernetes**: 컨테이너 오케스트레이션
- **Container Runtime**: 컨테이너 실행 환경

### PC2 기술 스택
- Ansible
- Firewalld
- MariaDB
- Keepalived
- Elasticsearch
- Kibana
- Filebeat
- Metricbeat

## 📁 프로젝트 구조

### PC1 프로젝트 구조
```
pc1/
├── inventory/           # 인벤토리 파일
│   └── hosts           # 호스트 정의 및 그룹 설정
├── playbooks/          # 플레이북
│   └── site.yml        # 메인 플레이북
├── vars/               # 변수 파일
│   └── common.yml      # 공통 변수 설정
└── roles/              # Ansible 역할
    ├── dns_server/     # DNS 서버 구성
    ├── mail_server/    # 메일 서버 구성
    ├── k8s_nodes/      # 쿠버네티스 노드 구성
    └── k8s_master/     # 쿠버네티스 마스터 구성
```

### PC2 프로젝트 구조
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

### PC1 하드웨어 요구사항
- **DNS 서버**
  - CPU: 2코어 이상
  - RAM: 2GB 이상
  - Storage: 20GB 이상

- **메일 서버**
  - CPU: 2코어 이상
  - RAM: 4GB 이상
  - Storage: 50GB 이상

- **쿠버네티스 마스터**
  - CPU: 4코어 이상
  - RAM: 8GB 이상
  - Storage: 50GB 이상

- **쿠버네티스 워커**
  - CPU: 4코어 이상
  - RAM: 8GB 이상
  - Storage: 100GB 이상

### PC2 하드웨어 요구사항
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

### PC1 설치 및 설정

#### 1. 사전 준비
```bash
# Ansible 설치
yum install -y ansible

# Git 클론
git clone [repository-url]
cd pc1
```

#### 2. SSH 키 설정
```bash
# SSH 키 생성
ssh-keygen -t rsa

# 각 서버에 SSH 키 복사
ssh-copy-id root@172.16.6.87  # DNS 서버
ssh-copy-id root@172.16.6.86  # 메일 서버
ssh-copy-id root@192.168.20.20  # 쿠버네티스 마스터
# 워커 노드들에도 동일하게 적용
```

#### 3. 변수 설정
`vars/common.yml` 파일을 수정하여 환경에 맞게 설정:
```yaml
# 도메인 설정
domain: project.com
domain_name: project.com

# DNS 설정
dns_server_ip: 172.16.6.87
dns_server_hostname: ns1

# 메일 서버 설정
mail_server_ip: 172.16.6.86
mail_server_hostname: mail
mail_user: mailuser
mail_dir: Maildir

# 쿠버네티스 설정
k8s_version: "1.30"
k8s_master_ip: 192.168.20.20
k8s_worker_ips:
  - 192.168.20.30
  - 192.168.20.40
  - 192.168.20.50
  - 192.168.20.60
  - 192.168.20.70
  - 192.168.20.80
```

#### 4. 실행
```bash
ansible-playbook -i inventory/hosts playbooks/site.yml
```

### PC2 설치 및 설정

#### 1. 사전 준비
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

#### 2. SSH 키 설정
```bash
# SSH 키 생성
ssh-keygen -t rsa -b 4096

# 공개키 복사
ssh-copy-id root@172.16.6.82
```

#### 3. Ansible 플레이북 실행
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

### PC1 상세 구성

#### DNS 서버 구성
- BIND 9 설치 및 설정
- 포워드/리버스 존 설정
- DNS 보안 설정
- 로깅 설정

#### 메일 서버 구성
- Postfix 설치 및 설정
- Dovecot 설치 및 설정
- SSL/TLS 설정
- 스팸 필터링 설정
- 메일 사용자 관리

#### 쿠버네티스 클러스터 구성
##### 마스터 노드
- 컨트롤 플레인 구성
- etcd 설정
- API 서버 설정
- 스케줄러 설정
- 컨트롤러 매니저 설정

##### 워커 노드
- kubelet 설정
- kube-proxy 설정
- 컨테이너 런타임 설정
- 네트워크 플러그인 설정

### PC2 상세 구성

#### 방화벽
- Firewalld 설치 및 설정
- 포트 포워딩 설정
- NAT 마스커레이딩 설정
- ICMP 및 TCP/UDP 포트 설정

#### DB 방화벽
- Firewalld 설치 및 설정
- DB 포트 포워딩 설정
- NAT 마스커레이딩 설정
- 보안 정책 적용

#### DB 서버
- MariaDB Master-Slave 구성
- Keepalived 설정
- 복제 사용자 설정
- 보안 설정

#### 백업 시스템
- Restic 설치 및 설정
- 백업 저장소 초기화
- 백업 스케줄 설정

#### SIEM 시스템
- Elasticsearch 설치 및 설정
- Kibana 설치 및 설정
- 인증 설정
- 대시보드 설정

#### 로그 전송 시스템
- Filebeat 설치 및 설정
- Metricbeat 설치 및 설정
- 시스템 모듈 설정
- 로그 전송 설정

## ⚠️ 주의사항

### PC1 주의사항
1. **실행 전 확인사항**
   - 모든 서버가 SSH로 접근 가능한지 확인
   - 방화벽 설정이 적절한지 확인
   - 디스크 공간이 충분한지 확인

2. **중요 설정**
   - 중요한 설정 변경 전 반드시 백업
   - 프로덕션 환경에서는 테스트 후 적용
   - 비밀번호 및 인증 정보는 안전하게 관리

3. **문제 해결**
   - 로그 파일 확인: `/var/log/messages`, `/var/log/ansible.log`
   - 서비스 상태 확인: `systemctl status [서비스명]`
   - 네트워크 연결 확인: `ping`, `telnet`, `netstat`

### PC2 주의사항
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

### PC1 변경 이력
- 2024-04-03: 초기 버전 릴리스
  - 기본 인프라 구성 추가
  - DNS 서버 구성 추가
  - 메일 서버 구성 추가
  - 쿠버네티스 클러스터 구성 추가

### PC2 변경 이력
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
