---
layout: post
title:  "Ubuntu_ HDD 추가, RAID, Shell script"
author: fennec-fox
tags: ubuntu
subtitle: "HDD 추가, RAID, shell script 작성과 실행"
category:  blog



---

<br>

# 1. HDD추가하기

1) HDD의 이름설정

- 디스크이름 : /dev/sda, /dev/sdb...
- 논리적인 파티션의 이름 : /dev/sda1, /dev/sda2 ….
- 파티션을 그냥 사용 할 수는 없으며, 반드시 특정한 디렉터리에 마운트 시켜주어야만 한다.

2) HDD추가할 때 전체 흐름

- 물리적인 HDD추가 —> fdisk(파티션 나뉨 /dev/sdb1) ——>mkfs(ext4로 파일 형식만들기) —> mount

- `/etc/fstab` 에 디스크를 등록해준다. 그래야 재부팅을 해도 디스크가 연결이 되어있다. 

  ```bash
  # FileSystem  MountPoint  Type	Option		Dump  pass
  /dev/sdb1			/mydata			ext4	defaults	0			0
  
  ```

  <br>

# 2. 여러개의 HDD를 하나처럼 이용하기

1) RAID의 정의 : 여러 개의 하드디스크를 하나의 디스크처럼 사용 하는 방식이다. 

하드웨어 RAID, 소프트웨어 RAID로 나눌 수 있다.

<br>

2) RAID 레벨 설명 —>  [scrolldowns님의 블로그](https://m.blog.naver.com/PostView.nhn?blogId=scrolldown&logNo=220981477416&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

<br>

# 3. Shell

### 1) 우분투의 기본 shell : bash shell

- Alias : 명령어 단축기능
- History : 화살표기능
- 연산기능
- 자동완성 기능(tab키)

### 2) 명령문 처리 방법

- (프롬프트) 명령어 [ 옵션 ] [ 인자 ]

  ```bash
  # rm -rf /mydir
  
  ```

- 환경변수 확인 : `echo $[환경 변수 이름]`

  ```bash
  echo $HOSTNAME
  
  ```

  위와 같이 사용하고, 많은 인자들이 있다. 

  여기에서 확인해보자 ==> [through.kim's 블로그](http://throughkim.kr/2016/12/22/linux-4/)

<br>

# 4. shell script 작성과 실행

```bash
[name.sh]

#!/bin/bash       #첫 행에 꼭 써주어야 한다.

echo "사용자 이름 : "$USER  # echo는 화면에 출력하라는 의미 
echo "홈 디렉터리 : "$HOME  # $USER, $HOME 는 환경변수
exit 0  # 마지막에 실행에 성공했다는 의미를 가지고 있음(없어도 실행은 됨) 

```

- 터미널에서 실행시키기

```bash
# 명령어로 실행
$ sh name.sh  

사용자 이름 : root 
홈 디렉터리 : /root

```

```bash
# 실행권한 부여해서 바로 실행하기

$ chmod u+x name.sh
$ ./name.sh

사용자 이름 : root 
홈 디렉터리 : /root

```

