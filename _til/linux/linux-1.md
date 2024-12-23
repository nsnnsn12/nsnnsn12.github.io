---
title: '리눅스 서버 구축 1(리눅스 설치)'
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- Linux

# table of contents
toc: true # 오른쪽 부분에 목차를 자동 생성해준다.
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
---

## 목표

1. 남는 노트북을 Linux 서버로 사용한다.
2. 해당 서버에 CICD 파이프라인을 구축한다.
3. 헤딩 서버에 Vs code server를 구축한다.

## 이유

1. Cloud로 넘어가기 전에 실제 서버를 구축해봄으로 경험을 쌓는다.
2. 노트북이 남는다.
3. 네트워크 공부를 실전적으로 하고 싶다.
4. Vscode server를 이용하여 원격 개발 환경을 구축하고 싶다.

## 리눅스 설치

> 참고 자료

- <https://docs.rockylinux.org/guides/installation/>

### booting usb 만들기

### install하기

### network 설정하기

<https://docs.rockylinux.org/guides/network/basic_network_configuration/#ip-address-changing-with-nmcli>

### sudo 권한 처리
<https://starseeker711.tistory.com/176>

### vi, vim 명령어 정리
<https://stricky.tistory.com/135>

### SSH 설정하기
<https://jumpcloud.com/blog/how-to-configure-secure-ssh-server-rocky-linux>

#### Step 1: SSH를 이용하여 원격 서버 접속하기

Rocky Linux를 설치 시 기본으로 OpenSSH가 설치된다.  
고로 SSH를 이용하여 원격 접속할 수 있다.

SSH 원격 접속을 위해 SSH client가 필요하다.  
나는 윈도우에서 SSH 원격 접속을 하기 위해 MobaXterm을 사용하였다.

- 터미널을 이용한 SSH 접근하는 방법

``` bash
ssh username@server_ip_address
-- username: 접근하고자 하는 계정
-- servier_ip_address: 접근하고자 하는 서버 IP
```

- MobaXterm을 이용한 SSH 접근 방법

![MobaXterm](/assets/images/linux/ssh1.png "MobaXterm을 이용한 SSH 접근 방법")

#### Step 2: SSH 기본 포트 변경

22번 port는 SSH 기본 포트로 잘 알려져 있으므로 변경하는 것이 좋다.  

1 설정 파일 변경 전 백업을 위해 복사본 만들기

``` bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config_backup
```

2 SSH 설정 파일 내의 포트 변경하기

``` bash
sudo sudo vi /etc/ssh/sshd_config
```

![Modify SSH Port](/assets/images/linux/ssh2.png "SSH 포트 변경")

3 SELinux에서 SSH 포트 변경하기

SELinux 설정을 변경하기 위해 semanage command가 설치되어 있는지 확인

``` bash
yum provides /usr/sbin/semanage
```

semanage를 이용하여 SSH 포트 변경

``` bash
sudo semanage port -a -t ssh_port_t -p tcp 원하는_포트_번호
```

4 방화벽 설정

``` bash
sudo firewall-cmd –-zone=public –-add-port=SSH_포트/tcp –-permanent
```

5 방화벽 재실행

```bash
sudo firewall-cmd –-reload
```

6 SSH 재실행

```bash
sudo systemctl restart sshd
```

7 새로운 포트로 SSH 접근 확인

```bash
ssh -p 설정한_SSH_PORT username@server_ip_address
```

### 노트북 닫았을 때도 실행 상태유지 하기

```bash
-- 관련 config 설정 수정
sudo gedit /etc/systemd/logind.conf
```

``` yaml
옵션 ignore 처리
HandleLidSwitch=ignore
```

```bash
-- config 설정 반영
systemctl restart systemd-logind.service
```

### 가상화 설정하기

### 방화벽 설정하기
