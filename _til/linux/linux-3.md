---
title: '리눅스 서버 구축 3(SSH 서버 설정)'
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


## 3. SSH 설정하기

### SSH란 무엇인가?

SSH = Secure Shell  
SSH는 암호화 프로토콜로 원격 서버와의 통신을 보호해준다.  
주로 원격 서버 접근이나 데이터 전송을 위해 사용한다.

### SSH 동작 방식

SSH는 공개키를 이용하여 클라이언트와 서버 간의 통신을 보호한다.

1. 클라이언트가 SSH 서버에 접근한다.
2. 통신이 처음 연결될 때 클라이언트와 서버는 키를 교환한다.  
    통신을 암호화하기 위한 키를 설정한다고 생각하면 될 것 같다.
3. 키 교환이 성공한 다음, 클라이언트는 인증 절차를 거친다.
   1.  인증 방식은 패스워드 기반, 공개키 인증 등등이 있다.
4.  인증 후 클라이언트와 서버 간의 secure channel이 구축된다.  
    그리고 클라이언트와 서버 간의 모든 통신은 암호화된다.

### Step 1: SSH를 이용하여 원격 서버 접속하기

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

### Step 2: SSH 기본 포트 변경

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

8 루트 권한 로그인 비활성화 및 접근 유저 제한

```bash
sudo vi /etc/ssh/sshd_config
```

PermitRootLogin 찾아 옵션 no로 바꾸기

맨 마지막 줄로 내려가서 유저 제한 설정 추가하기  
AllowUsers {username}

9 ssh key 설정 및 password login 제한하기

- ssh public and private key 발급
  - 필자는 MobaXterm을 사용하여 발급했다.
- public key 복사하여 authorized_keys 파일에 붙여 넣기, 없으면 새로 만들기
  - 경로: ~/.ssh/authorized_keys
- MobaXterm session 생성 시 use private key 체크

10 패스워드 로그인 비활성화

```bash
sudo vi /etc/ssh/sshd_config

-- PubKeyAuthentication yes
-- PasswordAuthentication no

-- 수정한 sshd_config 파일 적용
sudo systemctl restart sshd
```

>참고자료
<https://jumpcloud.com/blog/how-to-configure-secure-ssh-server-rocky-linux>