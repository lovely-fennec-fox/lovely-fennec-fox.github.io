---
layout: post
title:  "Ubuntu_ network 명령어"
author: fennec-fox
tags: ubuntu
subtitle: "nm-connection-editor, systemctl, ifconfig, nslookup, ping"
category:  ubuntu


---

<br>

# 리눅스 네트워크

### 1) 관련 명령어

- `nm-connection-editor` : network아이콘을 눌렀을 때 나오는 설정창을 불러옴

  nm = Network Manager의 약자로 네트워크와 관련된 작업 대부분은 이 명령을 기반으로 삼아 실행 할 수 있다. 

- `systemctl start/stop/restart/status networking` : 네트워크 설정을 변경한 후에 변경된 내용을 시스템에 저용시키는 명령

- `ifconfig [장치 이름]` : 해당 장치의 IP주소와 관련 정보를 출력해주는 명령어

- `nslookup` : DNS서버의 작동을 테스트하는 명령어

- `ping IP` : 해당 컴퓨터가 네트워크상에서 응답하는지를 테스트하는 간편한 명령어

### 2) 네트워크 설정과 관련되 파일

- `/etc/resolv.conf` : DNS 서버의 정보와 호스트 이름이 들어있는 파일
- `/etc/hosts` : 현 컴퓨터의 호스트 이름과 FQDN이 들어있는 파일이다.
- `/etc/network/interfaces` : 네트워크 설정 파일

