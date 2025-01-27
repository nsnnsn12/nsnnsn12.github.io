---
title: '리눅스 서버 구축 4(가상화 설정)'
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

Cockpit을 이용하여 KVM을 사용한다.

## Cockpit이란

Cockpit은 리눅스 서버를 모니터링하고 관리하는 웹 UI 기반의 도구.

주요 특징

- 웹 콘솔을 통해 서버 자원 모니터링
- 9090 포트를 기본으로 사용
- 네트워크 구성, 시스템 서비스, 스토리지 설정 등을 웹에서 관리 가능

## KVM이란

KVM(Kernel-based Virtual Machine)은 리눅스 커널에 통합된 오픈소스 가상화 기술

주요 특징

- 리눅스 커널과 직접 통합되어 있어 높은 성능과 안정성을 제공

## Cockpit 설치

Cockpit은 Rocky Linux 설치 시 기본적으로 포함되어 있다.  
하지만 KVM을 지원하는 기능은 따로 설치가 필요하다.

```bash
-- KVM 지원 기능
dnf install -y cockpit-machines

-- KVM 설치치
dnf install -y libvirt
```

## Cockpit 및 KVM 기능 활성화

```bash
systemctl enable --now libvirtd cockpit.socket
```

해당 명령어 실행 후 http://ip_address:9090로 관리 페이지에 접근할 수 있다.

## 가상화 머신 생성

생성 방법은 UI가 직관적이기 때문에 별다른 설명없이 설치 가능하다.  
그래도 자세한 설명이 필요하다면 아래의 링크를 참고하자.  
<https://docs.rockylinux.org/guides/virtualization/cockpit-machines/>

## 용어 정리

- 인바운드와 아웃바운드
  - 트래픽에 네트워크 간에 이동하는 방향을 의미  
    인바운드: 트래픽이 들어오는 것(요청받는 것)
    아웃바운드: 트래픽이 나가는 것(요청하는 것)

>참고자료
<https://docs.rockylinux.org/guides/virtualization/cockpit-machines/>
<https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_virtualization/configuring-virtual-machine-network-connections_configuring-and-managing-virtualization>
<https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_networking/configuring-a-network-bridge_configuring-and-managing-networking#configuring-a-network-bridge-by-using-nmcli_configuring-a-network-bridge>