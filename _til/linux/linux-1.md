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

## 이유

1. Cloud로 넘어가기 전에 실제 서버를 구축해봄으로 경험을 쌓는다.
2. 노트북이 남는다.
3. 네트워크 공부를 실전적으로 하고 싶다.
4. Cloud는 돈이 든다.

> 참고 자료  
> <https://docs.rockylinux.org/guides/installation/>

## 1. booting usb 만들기

1. ISO 다운받기
    1. DVD ISO => 전체 설치 이미지
    2. Boot ISO => 최소 boot 이미지로 repository 접근 필요(즉, 인터넷 연결 필요)
    3. 최소한의 소프트웨어 패키지를 포함하는 이미지
2. 다운받은 ISO로 usb 부팅 디스크 만들기
    참고링크: <https://rufus.ie/ko/>

> rocky linux download 페이지  
> <https://www.rockylinux.org/download/>

## 2. install하기 (최소 설치)

## 3. sudo 권한 처리

### su와 sudo의 차이

su(Switch User)  
현재 계정을 로그아웃하지 않고 다른 계정으로 전환

sudo(SuperUser Do)  
현재 계정에서 단순히 root 권한 만을 빌리는 것
즉, 하나의 명령에 대하여 일시적으로 root 권한을 사용하는 것.

안전성을 위해 sudo를 사용하는 것이 권장된다.  
하지만 계정이 sudo 명령어를 사용하기 위해서는 권한을 허용해주는 절차가 필요하다.

/etc/sudoers 목록에 권한을 부여하고자 하는 계정을 추가한다.
```bash
-- root 계정 전환
su

-- sudoers 수정
sudo vi /etc/sudoers
-- sudoers 파일에 아래와 같이 계정을 추가한다.
-- (username) ALL=(ALL:ALL) ALL
```

### vi, vim 명령어 정리

<https://stricky.tistory.com/135>

## 4. 노트북 닫았을 때도 실행 상태유지 하기

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
