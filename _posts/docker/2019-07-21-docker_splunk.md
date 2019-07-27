---
layout: post
title: "Docker를 활용한 Splunk 구성"
date: 2019-07-21 06:37:13 -0400
background: '/img/posts/05.jpg'
---

## *Docker & Splunk 구성 참고사이트* 
`SITE : `[Using Splunk with Traefik](https://medium.com/@nzmebooks/using-splunk-with-traefik-e41b8ae1537a)

---

```
mkdir -p /opt/splunk
touch /home/xbb123/splunk-launch.conf
```

The splunk-launch.conf will look like:
```
#   Version 7.2.4
# Modify the following line to suit the location of your Splunk install.
# If unset, Splunk will use the parent of the directory containing the splunk
# CLI executable.
#
# SPLUNK_HOME=/opt/splunk-home
# By default, Splunk stores its indexes under SPLUNK_HOME in the
# var/lib/splunk subdirectory.  This can be overridden
# here:
#
# SPLUNK_DB=/opt/splunk-home/var/lib/splunk
# Splunkd daemon name
SPLUNK_SERVER_NAME=Splunkd
# Splunkweb daemon name
SPLUNK_WEB_NAME=splunkweb
# If SPLUNK_OS_USER is set, then Splunk service will only start
# if the 'splunk [re]start [splunkd]' command is invoked by a user who
# is, or can effectively become via setuid(2), $SPLUNK_OS_USER.
# (This setting can be specified as username or as UID.)
#
# SPLUNK_OS_USER
# https://answers.splunk.com/answers/306998/why-am-i-getting-homepathoptsplunkvarlibsplunkaudi.html?childToView=578312#answer-578312
OPTIMISTIC_ABOUT_FILE_LOCKING=1
```


## splunk sh1
docker run \
    -p 8000:8000 \
    -p 8089:8089 \
    -p 9997:9997 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /home/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    --name splunk-sh1 \
    splunk/splunk:latest

## splunk lbf
docker run \
    -p 8001:8000 \
    -p 8091:8089 \
    -p 9998:9997 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /home/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    --name splunk-lbf \
    splunk/splunk:latest    

## docker idx1, idx2
docker run \
    -p 8002:8000 \
    -p 8092:8089 \
    -p 9999:9997 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /home/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    --name splunk-idx1 \
    splunk/splunk:latest

docker run \
    -p 8003:8000 \
    -p 8093:8089 \
    -p 10000:9997 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /home/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    --name splunk-idx2 \
    splunk/splunk:latest


## 기본형태
docker run \
    -p 8001:8000 \
    -p 8089:8089 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /home/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    splunk/splunk:latest


## docker command
docker exec -it splunk-sh1 /bin/bash

## docker 다운로드
docker pull splunk/splunk:latest

## docker windows
docker run \
    -p 8000:8000 \
    -p 8089:8089 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /c/Users/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    --name splunk-sh1 \
    splunk/splunk:latest

docker run \
    -p 8001:8000 \
    -p 8091:8089 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /c/Users/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    --name splunk-idx1 \
    splunk/splunk:latest

docker run \
    -p 8002:8000 \
    -p 8092:8089 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /c/Users/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    --name splunk-idx2 \
    splunk/splunk:latest

docker run \
    -p 8003:8000 \
    -p 8093:8089 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /c/Users/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    --name splunk-lbf \
    splunk/splunk:latest       

<br>
## !docker 실행 안됨
---
docker run -d -p 8000:8000 -e "SPLUNK_START_ARGS=--accept-license" -e "admin:bitek01!" --name splunk-sh1 splunk/splunk:latest

docker run -it -p 8000:8000 -e "SPLUNK_PASSWORD=bitek01!" -e "SPLUNK_START_ARGS=--accept-license" --name splunk-sh1 splunk/splunk:latest

docker run -it -p 8000:8000 -e "SPLUNK_PASSWORD=bitek01!" -e "SPLUNK_START_ARGS=--accept-license" --name splunk-sh1 splunk/splunk:latest


docker run -it -p 8000:8000 -e 'SPLUNK_START_ARGS=--accept-license' -e "SPLUNK_PASSWORD=bitek01!" --name splunk-sh1 splunk/splunk

docker run \
    -p 8000:8000 \
    -p 8088:8088 \
    -e 'SPLUNK_START_ARGS=--accept-license --no-prompt --answer-yes' \
    -e 'SPLUNK_USERNAME=admin' \
    -e 'SPLUNK_PASSWORD=bitek01!' \
    -v /home/xbb123/splunk-launch.conf:/opt/splunk/etc/splunk-launch.conf \
    splunk/splunk:latest
