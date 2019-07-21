## 컨테이너  
---
#### 라이프 사이클
* [`docker run [이미지 이름 또는 ID]`](http://docs.docker.io/en/latest/commandline/cli/#run) 컨테이너를 생성한다.
* [`docker stop [이미지 이름 또는 ID]`](http://docs.docker.io/en/latest/commandline/cli/#stop) 컨테이너를 정지시킨다.
* [`docker start [이미지 이름 또는 ID]`](http://docs.docker.io/en/latest/commandline/cli/#start) 컨테이너를 다시 실행시킨다.
* [`docker restart [이미지 이름 또는 ID]`](http://docs.docker.io/en/latest/commandline/cli/#restart) 컨테이너를 재가동한다.
* [`docker rm [이미지 이름 또는 ID]`](http://docs.docker.io/en/latest/commandline/cli/#rm) 컨테이너를 삭제한다.
* [`docker attach [이미지 이름 또는 ID]`](http://docs.docker.io/en/latest/commandline/cli/#attach) 실행중인 컨테이너에 접속한다.

#### 관련된 정보를 출력해주는 명령어
* [`docker ps`](http://docs.docker.io/en/latest/commandline/cli/#ps) 명령어는 실행중인 컨테이너 목록을 보여준다.
    __docker ps -a__ 실행중인 컨테이너와 멈춰있는 컨테이너를 모두 보여준다.
* [`docker inspect`](http://docs.docker.io/en/latest/commandline/cli/#inspect) ip 주소를 포함한 특정 컨테이너에 대한 모든 정보를 보여준다.
* [`docker logs`](http://docs.docker.io/en/latest/commandline/cli/#logs) 컨테이너로부터 로그를 가져온다.
* [`docker events`](http://docs.docker.io/en/latest/commandline/cli/#events) 컨테이너로부터 이벤트를 가져온다.
* [`docker port`](http://docs.docker.io/en/latest/commandline/cli/#port) 컨테이너의 특정 포트가 어디로 연결되어있는지 보여준다.
* [`docker top`](http://docs.docker.io/en/latest/commandline/cli/#top) 컨테이너에서 실행중인 프로세스를 보여준다.
* [`docker diff`](http://docs.docker.io/en/latest/commandline/cli/#diff) 컨테이너 파일 시스템에서 변경된 파일들을 보여준다.

#### File Copy
* [`docker cp  [호스트 경로] [이미지 이름 또는 ID]:[이미지 내 경로]`](http://docs.docker.io/en/latest/commandline/cli/#cp) 호스트의 파일을 컨테이너 내로 복사한다.  
* [`docker cp [이미지 이름 또는 ID]:[이미지 내 경로] [호스트 경로]`](http://docs.docker.io/en/latest/commandline/cli/#cp) 컨테이너 내의 파일을 호스트로 복사한다.  


## Docker 이미지
---
#### 라이프 사이클
* [`docker images`](http://docs.docker.io/en/latest/commandline/cli/#images) 모든 이미지 목록을 보여준다.
* [`docker build`](http://docs.docker.io/en/latest/commandline/cli/#build) Dockerfile을 통해 이미지를 생성한다.
* [`docker commit`](http://docs.docker.io/en/latest/commandline/cli/#commit) 컨테이너에서 이미지를 생성한다.
* [`docker rmi`](http://docs.docker.io/en/latest/commandline/cli/#rmi) 이미지를 삭제한다.
* [`docker insert`](http://docs.docker.io/en/latest/commandline/cli/#insert) URL에서 이미지로 파일을 집어넣는다.
* [`docker load -i`](http://docs.docker.io/en/latest/commandline/cli/#load) 표준 입력으로 tar 파일에서 (이미지와 태그를 포함한) 이미지를 불러온다.
* [`docker save -o`](http://docs.docker.io/en/latest/commandline/cli/#save) 모든 부모 레이어와 태그, 버전 정보를 tar 형식으로 표준출력을 통해 저장

#### 관련된 정보를 출력해주는 명령어
* [`docker history`](http://docs.docker.io/en/latest/commandline/cli/#history) 이미지의 이력 정보를 보여준다.
* [`docker tag`](http://docs.docker.io/en/latest/commandline/cli/#tag) 이미지에 이름으로 태그를 붙여준다(local 혹은 registry).

#### 마지막에 실행된 컨테이너의 ID
```
alias dl='docker ps -l -q'
docker run ubuntu echo hello world
docker commit `dl` helloworld
```
#### 명령어와 함께 커밋하기
```
docker commit -run='{"Cmd":["postgres", "-too -many -opts"]}' `dl` postgres
```
#### IP address 정보
```
docker run -i -t ubuntu /bin/bash
```
명령어를 사용하거나 아래 명령어를 사용합니다.
```
wget http://stedolan.github.io/jq/download/source/jq-1.3.tar.gz
tar xzvf jq-1.3.tar.gz
cd jq-1.3
./configure && make && sudo make install
docker inspect `dl` | jq -r '.[0].NetworkSettings.IPAddress'
```

#### 이미지의 환경 변수 읽어오기
```
docker run -rm ubuntu env
```
#### 오래된 컨테이너들 삭제하기
```
docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs docker rm
```
#### 멈춰있는 컨테이너들 삭제하기
```
docker rm `docker ps -a -q`
```
#### 이미지의 의존관계 이미지로 출력하기
```
docker images -viz | dot -Tpng -o docker.png
```
