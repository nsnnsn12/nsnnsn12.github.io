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

## 목표

이전에 설치한 Linux 서버를 외부에서 접근 가능하게 만든다.

## IP 할당하기

네트워크 통신을 하기 위해서는 IP가 필요하다.  
IP는 쉽게 생각하면 집 주소라고 생각하면 편하다.  
자신이 원하는 서버의 IP에 정보를 요청을 하고  
요청받은 서버는 Client의 IP로 요청 정보를 전달한다.

네트워크는 일반적으로 고정 IP 할당과 동적 IP 할당으로 나뉘어져 있다.

### 고정 IP 할당

- 정의
  - 고정 IP는 특정 장치에 항상 동일한 IP 주소를 수동으로 할당하는 방식.
  - 네트워크 관리자가 직접 설정하거나, DHCP(Dynamic Host Configuration Protocol)에서 특정 장치에 고정 IP를 예약하여 사용.

- 특징
  - 주소 변경 없음: 장치가 네트워크에 연결될 때마다 동일한 IP 주소를 사용.
  - 수동 관리 필요: IP 주소를 수동으로 입력하거나 DHCP를 통해 설정.

- 사용 사례
  - 서버: 웹 서버, 데이터베이스 서버 등 네트워크에 항상 동일한 IP 주소가 필요.
  - 프린터/네트워크 장치: 공유 프린터, 네트워크 스토리지 장치 등.
  - 원격 접속 장치: 고정 IP가 필요한 CCTV, IoT 장치 등.

### 동적 IP 할당 (Dynamic IP Allocation)

- 정의
  - 동적 IP는 DHCP 서버가 장치가 네트워크에 연결될 때마다 사용 가능한 IP를 자동으로 할당하는 방식.

- 특징
  - 자동 할당: 네트워크에 연결될 때마다 DHCP 서버에서 IP를 임시로 제공.
  - 임시성: IP 주소는 일정 시간이 지나거나 네트워크 연결이 끊기면 변경될 수 있음.

- 사용 사례
  - 일반 사용자 장치: PC, 스마트폰, 태블릿 등 이동성이 높은 장치.
  - Wi-Fi 네트워크: 카페, 호텔 등의 공용 네트워크.
  - 규모가 큰 네트워크: 기업 환경에서 많은 장치를 쉽게 관리하기 위해 동적 IP를 활용.

외부에서 접근 가능하도록 만들어야 하기 때문에 고정 아이피를 할당한한다.

## 서버 Network 설정하기

> 참고 자료  
> <https://docs.rockylinux.org/guides/network/basic_network_configuration/#ip-address-changing-with-nmcli>

필자는 Rocky Linux를 사용하고 있으며 nmtui 커맨드를 이용하여 네트워크를 설정하였다.  
nmtui는 네트워크 설정을 위한 화면을 제공해주기 때문에 편하게 이용할 수 있다.  

![MobaXterm](/assets/images/linux/network%20setting%201.png "nmtui를 이용한 고정 IP 할당")

네트워크 설정 변경 후 네트워크를 재기동시킨다.  
필자는 아래의 명령어를 이용하였다.  

``` bash
nmcli con down {network name} && nmcli con up {network name}
```

## 서버 Network 연결 확인

1 Gateway 접근 확인

``` bash
ping -c3 192.168.1.1 --본인 gateway ip 입력력
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=0.437 ms
64 bytes from 192.168.1.1: icmp_seq=2 ttl=64 time=0.879 ms
64 bytes from 192.168.1.1: icmp_seq=3 ttl=64 time=0.633 ms
```

2 LAN 라우팅 확인

``` bash
ping -c3 192.168.1.10
PING 192.168.1.10 (192.168.1.10) 56(84) bytes of data.
64 bytes from 192.168.1.10: icmp_seq=2 ttl=255 time=0.684 ms
64 bytes from 192.168.1.10: icmp_seq=3 ttl=255 time=0.676 ms
```

위에서 접근하는 IP는 본인 LAN에 연결되어 있는 IP 중 하나를 선택하면 된다.  
본인 공유기에 연결되어 있는 컴퓨터 중 하나의 IP를 찾아 넣으면 될 것이다.

3 DNS 서버 확인

``` bash
ping -c3 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=119 time=19.8 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=119 time=20.2 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=119 time=20.1 ms
```

4 DNS 동작 확인

``` bash
ping -c3 google.com
PING google.com (172.217.4.46) 56(84) bytes of data.
64 bytes from lga15s46-in-f14.1e100.net (172.217.4.46): icmp_seq=1 ttl=119 time=14.5 ms
64 bytes from lga15s46-in-f14.1e100.net (172.217.4.46): icmp_seq=2 ttl=119 time=15.1 ms
64 bytes from lga15s46-in-f14.1e100.net (172.217.4.46): icmp_seq=3 ttl=119 time=14.6 ms
```

## 외부에서의 접근을 위해 포트포워딩 설정하기

### 외부에서의 접근을 위해서는 공인 IP가 필요하다

위에서의 고정 IP 할당은 LAN 내의 고정 IP를 할당한 것이다.  
이말은 공유기에 연결되어 있는 컴퓨터들끼리는 설정한 IP를 이용하여 서버에 접근할 수 있지만  
외부에서는 접근할 수 없다는 것을 의미한다.

이를 위해 IP를 통해 어떻게 요청과 응답을 받는지에 대한 네트워크 이해가 필요하다.  
추후 해당 내용을 추가로 정리할 예정이다.

### 어떻게 공인 IP를 할당할 수 있을까?

**일단 개인이 공인 IP를 받기 위해서는 기업용 인터넷을 사용해야 한다.**  
한마디로 돈이 많이든다.

**내가 하고자 하는 것은 공인 IP와 내가 설정한 고정 IP를 매핑시키는 것이다.**  
우리가 주로 사용하는 Iptime 공유기의 경우 포트포워딩 설정이 가능하다.  

**포트포워딩이란**  
외부에서 우리가 사용하고 있는 공유기의 접근하면 공유기에서 전달받은 포트를 기준으로 IP를 매핑시켜준다.  
![Port Forwarding](/assets/images/linux/port%20forwarding.jpg "Port Forwarding을 이용한 IP 매핑")

### ipTIME을 이용한 포트포워딩 설정

>아래 사이트를 참고하여 포트포워딩 설정  
><https://luckygg.tistory.com/270>

### DDNS 설정

우리가 사용하고 있는 공유기 역시 IP가 동적으로 할당된다.  
고로 매번 내가 설치한 서버에 접근하기 위해서는 공유기가 할당받은 IP 정보를 확인해야 한다.  
이런 번거로움을 없애기 위해 DDNS를 사용한다.  

#### DDNS란?

네트워크의 IP 주소가 변경될 때 자동으로 도메인 이름과 연결된 IP 주소를 업데이트하는 기술이다.  
주로 동적 IP 주소를 사용하는 환경에서 사용되며, 고정 IP 없이도 도메인을 통해 네트워크나 서버에 안정적으로 접근할 수 있게 만든다.  
**그리고 ipTIME은 DDNS 또한 지원한다.**

>아래 사이트를 참고하여 DDNS 설정  
><https://luckygg.tistory.com/271>