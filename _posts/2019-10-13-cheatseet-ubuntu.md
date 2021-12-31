---
layout: post
title: CheatSeet_ ubuntu
description: CheatSeet_ ubuntu
img: post-8.jpg
tags: [CheatSeet, Ubuntu]
author: fennec-fox
category:  cheat-sheet
---

<br>

# 1. Ubuntu 설치

<br>

#### 1) 설치 공간

```

- 우분투 설치를 위해 기본적으로 루트 파티션과 스왑 영역이 필요하다.
- 루트(/)파티션은 다른 모든 시스템 디렉터리(/home, /usr 등)를 포함하는 최상위 파티션이며, 루트 파티션이 존재하지 않으면 부팅이 불가능하므로 반드시 할당해야 한다.
- 스왑 영역은 실제 물리적인 메모리가 가득 찰 때, 하드디스크를 메모리처럼 사용하기 위해 필요한 공간이다.
일반적으로 시스템 메모리가 4GB~16GB이면, 스왑 공간을 최소 4GB를 권장한다.

```

<br>

#### 2) 시스템 디렉터리

- `/home` : 사용자 자료가 저장되는 디렉터리
- `/usr` : 시스템, 응용 프로그램에 필요한 파일들이 저장되는 디렉터리
- `/var` : 전자메일, 시스템 로그 등 지속적으로 늘어나는 임시 자료를 저장하는 디렉터리
- `/tmp` : 임시로 파일을 생성 또는 삭제하는 디렉터리

<br>

#### 3) locale 설정하기

- 리눅스에서 시스템의 국가, 언어, 날짜/시간 형식, 시간대를 비롯한 설정은 locale로 정의합니다.
- 우분투의 데스크 탑은 한글 로케일(ko_KR.UTF-8)로 설정하고, 우분투 서버는 영어로 설정하는 편이 좋습니다.

```bash

# locale 확인
$ locale

# locale 변경
$ vi /etc/default/locale

# 다음의 설정을 영어로 변경해준다.
# LC_ALL=en_US.UTF-8
# LANG=en_US.UTF-8

```

<br>

#### 4) 네트워크 설정하기

- 우분투는 네트워크 메니저가 네트워크 상태를 감지하고 자동으로 설정해 주지만, 네트워크의 설정을 자주 변경할 필요가 없을 때에는 

  네트워크 메니저가 가끔 자동으로 네트워크 설정을 바꾸기 때문에 관리자가 직접 설정파일에 주소 정보를 입력하는 편이 바람직하다.

- 패킷이 드나드는 장치를 네트워크 인터페이스 라고 한다. 네트워크 메니저를 삭제하면 루프백을 제외한 모든 네트워크 인터페이스가 해제 된다. 

```bash

# 네트워크 메니저 제거하기
$ apt-get remove -y --purge network-manager

# 네트워크 환경 설정 방법
$ vi /etc/network/interfaces
 
 # 1) 네트워크 주소 정보를 자동으로 받아올 때,
 #    eth0를 활성화 시키고 DHCP를 통해 네트워크 주소 정보를 자동으로 받아오도록 설정한다.
 'auto lo'
 
  'auto eth0'
  'iface eth0 inet dhcp'
  
  # 2) 네트워크 주소를 직접 입력할 때,
  #    eht0에 수동으로 네트워크 주소 정보를 할당 하고 IP주소, 넷마스크, 기본 게이트웨이주소, DNS주소를 입력해준다.
  'auto lo'
  
  'auto eth0'
  'iface eth0 inet static'
  '  address 192.168.0.2'
  '  netmask 255.255.255.0'
  '  gateway 192.168.0.1'
  '  dns-nameservers 8.8.8.8 8.8.4.4'

  # 3) 무선 네트워크 연결할 때,
  # 무선 네트워크 장치 확인
   $ iwconfig
   # wlan0 에 대한 정보를 확인한다. 


# 수정된 사항을 반영하기 위해 네트워크 인터페이스 eth0를 내렸다가 올려준다.
$ ifdown eth0
$ ifup eth0
  
```





