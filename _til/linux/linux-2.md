---
title: '리눅스 서버 구축 2(네크워크 설정)'
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

# 리눅스 서버 구축 2(네크워크 설정)

네트워크 통신을 하기 위해서는 IP가 필요하다.  
IP는 쉽게 생각하면 집 주소라고 생각하면 편하다.  
자신이 원하는 서버의 IP에 정보를 요청을 하고  
요청받은 서버는 Client의 IP로 요청 정보를 전달한다.

네트워크는 일반적으로 고정 IP 할당과 동적 IP 할당으로 나뉘어져 있다.

### 고정 IP 할당

**정의**  

 - 고정 IP는 특정 장치에 항상 동일한 IP 주소를 수동으로 할당하는 방식.
 - 네트워크 관리자가 직접 설정하거나, DHCP(Dynamic Host Configuration Protocol)에서 특정 장치에 고정 IP를 예약하여 사용.

**특징**

 - 주소 변경 없음: 장치가 네트워크에 연결될 때마다 동일한 IP 주소를 사용.
 - 수동 관리 필요: IP 주소를 수동으로 입력하거나 DHCP를 통해 설정.

**사용 사례**

 - 서버: 웹 서버, 데이터베이스 서버 등 네트워크에 항상 동일한 IP 주소가 필요.
 - 프린터/네트워크 장치: 공유 프린터, 네트워크 스토리지 장치 등.
 - 원격 접속 장치: 고정 IP가 필요한 CCTV, IoT 장치 등.

### 동적 IP 할당 (Dynamic IP Allocation)

**정의**  
동적 IP는 DHCP 서버가 장치가 네트워크에 연결될 때마다 사용 가능한 IP를 자동으로 할당하는 방식입니다.

**특징**

- 자동 할당: 네트워크에 연결될 때마다 DHCP 서버에서 IP를 임시로 제공.
- 임시성: IP 주소는 일정 시간이 지나거나 네트워크 연결이 끊기면 변경될 수 있음.

**사용 사례**

- 일반 사용자 장치: PC, 스마트폰, 태블릿 등 이동성이 높은 장치.
- Wi-Fi 네트워크: 카페, 호텔 등의 공용 네트워크.
- 규모가 큰 네트워크: 기업 환경에서 많은 장치를 쉽게 관리하기 위해 동적 IP를 활용.

필자의 경우 해당 외부에서 접근 가능한 서버를 만들어야 하기 때문에 고정 아이피를 할당했다.

## network 설정하기
> 참고 자료  
> <https://docs.rockylinux.org/guides/network/basic_network_configuration/#ip-address-changing-with-nmcli>

## 용어 정리

- 인바운드와 아웃바운드
  - 트래픽에 네트워크 간에 이동하는 방향을 의미  
    인바운드: 트래픽이 들어오는 것(요청받는 것)
    아웃바운드: 트래픽이 나가는 것(요청하는 것)


## 방화벽 설정하기