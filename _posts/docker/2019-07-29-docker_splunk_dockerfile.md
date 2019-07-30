---
layout: post
title: "Dockerfile를 활용한 Splunk 구성 #2"
date: 2019-07-21 06:37:13 -0400
background: '/img/posts/05.jpg'
---
# Docker Run
<br>



# 개요

모든 이미지 생성시 공통으로 사용할 Centos7 이미지를 생성한다.

이 이미지에는 미들웨어 또는 어플리케이션이 들어있지 않고 OS에서 기본적으로 필요한 Package만 설치한다.

Apache나 Tomcat 등 다른 이미지 생성시 이 이미지를 기본으로 하면, 추가적으로 필요한 내용만 설치하면되므로 중복작업을 제거할 수 있고 Docker파일을 보다 간결하게 만들 수 있다.

DockerHub에 있는 docker.io/centos:7 이미지를 다운로드 받아서 만들 예정이므로 인터넷에 연결 가능해야 한다.

  

# 1. Docker Image 생성을 위한 Directory 및 파일 구성

## 1) Directory생성

`xbb123@h57:~$` `cd`  `docker-images/`

`xbb123@h57:~/docker-images$` `mkdir`  `centos7-base`

`xbb123@h57:~/docker-images/centos7-base$` `cd`  `centos7-base/`

## 2) Dockerfile 생성

`xbb123@h57:~/docker-images/centos7-base$` `vi`  `Dockerfile`

**Dockerfile**

`FROM docker.io``/centos``:7.4.1708`

`# 사용자 지정`

`USER root`

`# 언어셋 설치`

`RUN yum clean all \`

`&& yum repolist \`

`&& yum -y update \`

`&&` `sed`  `-i` `"s/en_US/all/"`  `/etc/yum``.conf \`

`&& yum -y reinstall glibc-common`

`# 기본적으로 필요한 OS 패키지를 설치한다.`

`RUN yum -y` `install`  `tar`  `unzip` `vi`  `vim telnet net-tools curl openssl \`

`&& yum -y` `install`  `apr apr-util apr-devel apr-util-devel \`

`&& yum -y` `install`  `elinks` `locate`  `python-setuptools \`

`&& yum clean all`

`ENV LANG=ko_KR.utf8 TZ=Asia``/Seoul`
`# 포트`
`EXPOSE 8000 8089 8191 9997`
`# 설치`
`ADD splunk-7.3.0-657388c7a488-Linux-x86_64.tgz /opt`

`# 컨테이너 실행시 실행될 명령`

`CMD [``"/bin/bash"``]`

# 2. Image 생성

`# docker build -t [이미지명] .`

`xbb123@h57:~/docker-images/centos7-base$` `docker build -t centos7-base:7.4 .`

`Sending build context to Docker daemon 2.048 kB`

`Step 1 : FROM docker.io``/centos``:7`

`---> 0584b3d2cf6d`

`Step 2 : USER root`

`---> Running` `in`  `070a0180b355`

`---> ce587d6b302b`

`...`

`# 생성된 이미지 확인`

`xbb123@h57:~/docker-images/centos7-base$`  `docker images`

`REPOSITORY TAG IMAGE ID CREATED SIZE`

`user1-centos7-base 7.4 04df9befd9c6 About a minute ago 286.5 MB`

# 3. 컨테이너 실행

`# 컨테이너 실행하고 바로 터미널로 접속`

`xbb123@h57:~/docker-images/centos7-base$` `docker run -ti -p 8000:8000 --name=xbb123-centos7-base centos7-base:7.4`

`# 컨테이너 내부`

`[root@7048f9eb2833 /]``# hostname`

`7048f9eb2833`

`[root@7048f9eb2833 /]``# cat /etc/redhat-release`

`CentOS Linux release 7.2.1511 (Core)`

`[root@7048f9eb2833 /]``# ifconfig`

`eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500`

`inet 172.17.0.3 netmask 255.255.0.0 broadcast 0.0.0.0`

`inet6 fe80::42:acff:fe11:3 prefixlen 64 scopeid 0x20<link>`

`ether 02:42:ac:11:00:03 txqueuelen 0 (Ethernet)`

`RX packets 8 bytes 648 (648.0 B)`

`RX errors 0 dropped 0 overruns 0 frame 0`

`TX packets 8 bytes 648 (648.0 B)`

`TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0`

`lo: flags=73<UP,LOOPBACK,RUNNING> mtu 65536`

`inet 127.0.0.1 netmask 255.0.0.0`

`inet6 ::1 prefixlen 128 scopeid 0x10<host>`

`loop txqueuelen 1 (Local Loopback)`

`RX packets 0 bytes 0 (0.0 B)`

`RX errors 0 dropped 0 overruns 0 frame 0`

`TX packets 0 bytes 0 (0.0 B)`

`TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0`

OPTIMISTIC_ABOUT_FILE_LOCKING = 1

# 4. 컨테이너 종료

`# docker run -ti 로 실행한 경우 빠져나오면 바로 컨테이너가 종료됨`

`[root@7048f9eb2833 /]``# exit`

`exit`

`xbb123@h57:~/docker-images/centos7-base$` `sudo`  `docker` `ps`  `-a`

`CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES`

`7048f9eb2833 centos7-base` `"/bin/bash"` `About a minute ago Exited (0) 10 seconds ago user1-centos7-base`

`d25d3240a374 httpd` `"httpd-foreground"` `20 minutes ago Up 14 minutes 80``/tcp` `user1-httpd`

# 5. 컨테이너 삭제

`# 종료된 컨테이너 조회`

`xbb123@h57:~/docker-images/centos7-base$` `sudo`  `docker` `ps`  `-a`

`CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES`

`7048f9eb2833 centos7-base` `"/bin/bash"` `2 minutes ago Exited (0) About a minute ago user1-centos7-base`

`# 컨테이너 삭제`

`xbb123@h57:~/docker-images/centos7-base$` `sudo`  `docker` `rm`  `xbb123-centos7-base`

`xbb123-centos7-base`

# 6. splunk setting

  The splunk-launch.conf will look like:
  
`OPTIMISTIC_ABOUT_FILE_LOCKING=1`