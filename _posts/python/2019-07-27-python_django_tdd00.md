---
layout: post
title: "Python Django TDD #0"
subtitle: "TDD With Python"
date: 2019-07-27 10:45:13 -0400
background: '/img/posts/06.jpg'
tags: [tag1, tag2, tag3]
---

# 00. 준비하기

출처:

-   [Preface](http://www.obeythetestinggoat.com/book/preface.html)
-   [Prerequisites and Assumptions](http://www.obeythetestinggoat.com/book/pre-requisite-installations.html)

-   [서문](https://wikidocs.net/11056#_1)
-   [준비사항](https://wikidocs.net/11056#_2)
    -   [사전 지식](https://wikidocs.net/11056#_3)
    -   [운영체제 - 우분투](https://wikidocs.net/11056#-)
    -   [파이썬3와 virtualenv](https://wikidocs.net/11056#3-virtualenv)
        -   [가상환경 만들기](https://wikidocs.net/11056#_4)
        -   [가상환경 활성화](https://wikidocs.net/11056#_5)
        -   [가상환경 빠져나오기](https://wikidocs.net/11056#_6)
    -   [Django 1.11과 Selenium](https://wikidocs.net/11056#django-111-selenium)
        -   [현재 파이썬 패키지 설치 상황](https://wikidocs.net/11056#_7)
        -   [pip 업그레이드](https://wikidocs.net/11056#pip)
        -   [Django 및 Selenium 설치](https://wikidocs.net/11056#django-selenium)
        -   [Django 설치 확인](https://wikidocs.net/11056#django)
    -   [Git 설치](https://wikidocs.net/11056#git)
    -   [파이어폭스 브라우저와 Geckodriver 설치](https://wikidocs.net/11056#geckodriver)
    -   [PyCharm 커뮤니티 에디션](https://wikidocs.net/11056#pycharm)
        -   [설치 및 설정](https://wikidocs.net/11056#_8)
-   [요약 및 의견](https://wikidocs.net/11056#_9)

# 서문

Test-Driven Development with Python 교재는 크게 세 부분으로 나눌 수 있다.

-   1부 (1~7장): 기초
    
    TDD 기초, 기능 테스트(Selenium), Django 기초
    
-   2부 (8~17장): 웹 개발 핵심
    
    웹 개발 과정에서 TDD 적용
    
-   3부 (18~26장): 고급 주제
    
    Mock, 서드파티 시스템, 픽스처, CI 등
    

# 준비사항

## 사전 지식

1.  파이썬3 (!= 파이썬2)
2.  HTML 문법
3.  Django 기초
4.  자바스크립트

## 운영체제 - 우분투

파이썬 개발 환경은 윈도우보다는 리눅스 또는 맥OS를 추천한다.

그 이유는 쉘 명령어나 컴파일러 같은 여러 가지 빌드 도구를 윈도우보다 쉽게 설치 및 관리할 수 있기 때문이다.

리눅스는 무료 공개 운영체제이고 우분투는 국내에서 사용자가 가장 많은 배포판이기 때문에 문제가 생겼을 때 도움을 얻기 쉬운 장점이 있다.

남는 컴퓨터에 개발용 우분투를 설치할 수도 있지만 사용 중인 윈도우 컴퓨터에서 VirtualBox를 설치해 우분투 가상머신을 설치할 수도 있다.

## 파이썬3와 virtualenv

-   우분투는 호환성 문제 때문에 기본적으로 파이썬2와 파이썬3 모두 설치되어 있다.
-   파이썬2는  `python`  명령어로 실행할 수 있고 파이썬3는 명시적으로  `python3`  명령어로 실행한다.
-   신규 프로젝트를 시작한다면 파이썬3로 시작하자.

파이썬 버전 3.4부터  `virtualenv`  패키지를 별도로 설치하지 않고 사용할 수 있다. 그러나 우분투에서는 별도로 패키지 프로그램을 설치해야 한다.

```
sudo apt-get install python3-venv

```

파이썬 개발에서 가상환경을 쓰는 이유는 다음과 같다.

-   하나의 시스템에서 여러 개의 프로젝트를 진행할 때 서로 독립된 환경으로 구축할 수 있다.
-   가상환경 마다 다른 버전의 패키지를 설치할 수 있고 서로 충돌이 발생하지 않기 때문에 불필요한 문제를 야기하지 않는다.

### 가상환경 만들기

```
$ python3 -m venv venv

```

### 가상환경 활성화

```
$ source venv/bin/activate

```

### 가상환경 빠져나오기

```
(venv) $ deactivate

```

여기에서는 단순히  `virtualenv`를 설명하고 있고 교재에서는  `virtualenvwrapper`  도구를 추천하고 있다.

그러나 실무적으로는 이제  `pyenv`를 추천하고 싶다. 그러나 사용법이 상대적으로 복잡하여 여기서는 설명하지 않는다.

## Django 1.11과 Selenium

`pip`  명령어로 파이썬 패키지를 관리할 수 있다.

### 현재 파이썬 패키지 설치 상황

```
pip freeze

```

### pip 업그레이드

```
pip install --upgrade pip

```

### Django 및 Selenium 설치

```
pip install Django selenium

```

### Django 설치 확인

pip 패키지 설치 상태로 확인할 수 있다.

```
(venv) $ pip freeze
Django==1.11.5
pkg-resources==0.0.0
pytz==2017.2
selenium==3.5.0

```

파이썬 인터프리터 콘솔에서 확인할 수도 있다.

```
(venv) $ python
Python 3.5.2 (default, Nov 17 2016, 17:05:23) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import django
>>> django.VERSION
(1, 11, 5, 'final', 0)
>>> django.get_version()
'1.11.5'
>>> 

```

## Git 설치

우분투 패키지 프로그램 설치로 아래와 간단히 버전관리시스템 Git을 설치할 수 있다.

```
sudo apt-get install git

```

## 파이어폭스 브라우저와 Geckodriver 설치

파이어폭스는 우분투 데스크톱에 기본 설치되는 브라우저이다.

[Geckodriver](https://github.com/mozilla/geckodriver/releases)에서 Geckodriver를 다운로드한다.

Geckodriver는 다운로드 후 압축 풀면 파일이 생성되는데  `~/bin`  같은 적당한 위치에 넣고  `$PATH`  환경변수에 경로를 등록한다.

## PyCharm 커뮤니티 에디션

교재는 IDE 사용을 권장하지 않는다. 적어도 이 책을 학습하는 동안이라도 한 줄이라도 더 직접 코드 입력하고 명령하라는 의미이다.

여기서는 PyCharm 커뮤니티 에디션을 기준으로 설명한다. 프로페셔널 에디션은 Django 개발을 위해 여러 가지 편리한 기능을 제공하지만 커뮤니티 에디션은 Django 관한 기능을 지원하지 않는다. PyCharm 커뮤니티 에디션을 사용하면 어차피 Django 관련 명령어는 다 직접 수동으로 입력해야 한다.

파이썬 개발자가 가장 많이 사용하는 IDE 중 하나인 PyCharm을 익숙해지는 계기가 되도록 한다.

### 설치 및 설정

파일을 다운로드 해 압축을 풀면 설치는 끝이다.

압축을 풀어  `bin`  디렉토리 안에  `source pycharm.sh`  명령어로 실행할 수 있다. 그러나 매번 명령어를 입력해 실행하면 불편하므로 론처에 고정하도록 한다.(Lock to Launcher)

프로젝트에 알맞는 파이썬 가상환경 인터프리터를 설정하는 것을 잊지 않도록 한다.

이외 터미널 및 파이썬 콘솔에서 가상환경에 잘 진입하는지 확인한다.

# 요약 및 의견

Django 개발환경을 구축해본 경험이 있다면  `Geckodriver`와  `Selenium`  설치 외에 특이사항은 없다.

그러나 아무래도 쉘 명령어 작업이 많아서 윈도우보다는 리눅스/맥OS에서 작업을 권장한다.

출처: [https://wikidocs.net/11056](https://wikidocs.net/11056)