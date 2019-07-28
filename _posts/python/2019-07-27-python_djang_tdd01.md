---
layout: post
title: "Python Django TDD #1"
subtitle: "TDD With Python"
date: 2019-07-27 11:45:13 -0400
background: '/img/posts/06.jpg'
tags: [tag1, tag2, tag3]
---

# 01. Django 그리고 기능 테스트

출처:

-   [Getting Django Set Up Using a Functional Test](http://www.obeythetestinggoat.com/book/chapter_01.html)

-   [테스트 염소에 절대 복종하라.](https://wikidocs.net/11057#_1)
-   [Django 프로젝트 만들기](https://wikidocs.net/11057#django)
-   [첫 번째 기능 테스트](https://wikidocs.net/11057#_2)
    -   [기능 테스트 작성 및 테스트](https://wikidocs.net/11057#_3)
    -   [기능 테스트를 통과시키기](https://wikidocs.net/11057#_4)
-   [Git 저장소 준비 및 설정](https://wikidocs.net/11057#git)
-   [요약 및 의견](https://wikidocs.net/11057#_5)

# 테스트 염소에 절대 복종하라.

테스트 염소(Testing goat)는 파이썬 커뮤니티에서 TDD의 비공식적인 마스코트이다.

테스트 염소에 절대 복종하라는 말을 하는데 이는 개발 과정에서 무조건적으로 테스트를 작성하라는 뜻이다.

일반적으로 웹 프레임워크 개발의 첫 시작은  **다운로드, 설치, 설정 후 스크립트 실행**으로 설명한다.

반면에 TDD에서 개발의 첫 시작은 언제나  **테스트를 작성**하는 것이다.

테스트를 하는 이유는 코드가 올바르게 동작하는 것을 확인하는 것보다도 예상한대로 오류가 발생하는지 확인하는 것이다.

**만약 테스트가 단순히 코드가 올바르게 동작하는 것을 확인하는 것뿐이라면 테스트는 구현과 다를 바 없고 결국 테스트하지 않은 것이다.**

# Django 프로젝트 만들기

`superlists`라는 이름의 Django 프로젝트를 만든다.

```
$ django-admin.py startproject superlists

```

`superlists`  Django 프로젝트의 디렉토리/파일 구조는 아래와 같다.

```
superlists/
    manage.py
    superlists/
        __init__.py
        settings.py
        urls.py
        wsgi.py

```

# 첫 번째 기능 테스트

## 기능 테스트 작성 및 테스트

위와 같이 기본적으로 Django 프로젝트 기본 뼈대를 만들고 비로소 첫 번째 기능 테스트를 작성한다.

`superlists`  디렉토리 안에  `function_tests.py`  파일을 만든다.

`function_tests.py`  파일

```
from selenium import webdriver

browser = webdriver.Firefox()
browser.get('http://localhost:8000')

assert 'Django' in browser.title

```

위 코드의 내용은 다음과 같다:

-   Selenium  `webdriver`를 구동하여 실제로 파이어폭스 브라우저를 띄운다.
-   로컬 컴퓨터에서 웹 페이지가 열리는지 확인한다.
-   열린 웹 페이지에서  `Django`  문자열이 있는지 확인한다.

기능 테스트는 아래와 같이 실행한다.

```
$ python function_tests.py

```

기능 테스트 결과 아래와 같이 파이어폭스 브라우저가 실행되고  `Unable to connect`  오류가 발생한다.

![결과화면: Unable to connect](https://wikidocs.net/images/page/11057/001.PNG)

터미널 화면에서도 아래와 같이 연결 실패 에러를 확인할 수 있다.

```
$ python function_tests.py 
Traceback (most recent call last):
  File "function_tests.py", line 4, in <module>
    browser.get('http://localhost:8000')
  File "/home/mang/.pyenv/versions/venv-tdd-goat/lib/python3.6/site-packages/selenium/webdriver/remote/webdriver.py", line 309, in get
    self.execute(Command.GET, {'url': url})
  File "/home/mango/.pyenv/versions/venv-tdd-goat/lib/python3.6/site-packages/selenium/webdriver/remote/webdriver.py", line 297, in execute
    self.error_handler.check_response(response)
  File "/home/mango/.pyenv/versions/venv-tdd-goat/lib/python3.6/site-packages/selenium/webdriver/remote/errorhandler.py", line 194, in check_response
    raise exception_class(message, screen, stacktrace)
selenium.common.exceptions.WebDriverException: Message: Reached error page: about:neterror?e=connectionFailure&u=http%3A//localhost%3A8000/&c=UTF-8&f=regular&d=Firefox%20can%E2%80%99t%20establish%20a%20connection%20to%20the%20server%20at%20localhost%3A8000.

```

만약 이러한 오류가 확인되지 않는다면 Geckodriver와 Selenium이 올바로 설치되었는지 살펴본다.

이렇게 실패하는 테스트를 만들었고 이러한 실패하는 테스트를 만드는 것이 TDD에서는 개발의 출발점이다.

## 기능 테스트를 통과시키기

앞서 기능 테스트를 만들었지만 테스트 결과는 실패다. 이제 테스트 통과로 문제를 해결해나가야 한다.

기능 테스트가 실패한 이유는 서버를 구동하지 않은 상태에서 웹 페이지에 접근했기 때문이다. 따라서 테스트 서버를 구동하면 간단하게 문제를 해결할 수 있다.

```
$ python manage.py runserver

```

다른 터미널에서 기능 테스트를 재실행한다.

```
$ python function_tests.py 

```

이제 아래 결과화면과 같이 파이어폭스 브라우저가 올바로 실행되고 터미널에 아무런 에러 출력 없이 기능 테스트를 완료한다.

![결과화면: It worked!](https://wikidocs.net/images/page/11057/002.PNG)

# Git 저장소 준비 및 설정

파일을 정리하고 아래와 같이 저장소를 초기화한다.

```
$ cd superlists/
$ git init .
Initialized empty Git repository in /home/mango/Projects/tdd/superlists/.git/

```

`.idea`  디렉토리와  `db.sqlite3`,  `geckodriver.log`  파일은 저장소에 저장해서 버전관리할 필요가 없는 파일이므로 무시하도록  `.gitignore`  파일에 등록한다.

```
$ echo .idea/ >> .gitignore
$ echo db.sqlite3 >> .gitignore
$ echo geckodriver.log >> .gitignore

```

커밋하기 전에 현재 디렉토리의 파일을 추가한다.

```
$ git add .
(venv-tdd-goat) mango@vbox:~/Projects/tdd/superlists$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   .gitignore
    new file:   function_tests.py
    new file:   manage.py
    new file:   superlists/__init__.py
    new file:   superlists/__pycache__/__init__.cpython-36.pyc
    new file:   superlists/__pycache__/settings.cpython-36.pyc
    new file:   superlists/__pycache__/urls.cpython-36.pyc
    new file:   superlists/__pycache__/wsgi.cpython-36.pyc
    new file:   superlists/settings.py
    new file:   superlists/urls.py
    new file:   superlists/wsgi.py

```

위에서 보면 불필요한  `.pyc`  파일까지 추가(add)되어 staging area에 있다. 즉, 모두 커밋하기 직전이라서 이들은 삭제해야 한다.

```
$ git rm -r --cached superlists/__pycache__
rm 'superlists/__pycache__/__init__.cpython-36.pyc'
rm 'superlists/__pycache__/settings.cpython-36.pyc'
rm 'superlists/__pycache__/urls.cpython-36.pyc'
rm 'superlists/__pycache__/wsgi.cpython-36.pyc'

```

위와 같이  `--cached`  옵션을 주면 git 안에서 unstaged 상태(untracked 또는 modified)로 되돌릴 뿐 작업 디렉토리에서 파일을 진짜로 삭제하지는 않는다.

이제 이 디렉토리/파일들을  `.gitignore`에 등록해줘야 할 것이다.

```
$ echo __pycache__ >> .gitignore
$ echo *.pyc >> .gitignore

```

다시 git 로컬 저장소 상태를 살펴보면 다음과 같다.

```
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   .gitignore
    new file:   function_tests.py
    new file:   manage.py
    new file:   superlists/__init__.py
    new file:   superlists/settings.py
    new file:   superlists/urls.py
    new file:   superlists/wsgi.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   .gitignore

```

`.gitignore`  파일의 상태가  `modified`이므로 다시 추가해주고 커밋한다.

```
$ git add .gitignore
$ git commit

```

커밋 메시지를 입력하기 위해 nano 에디터에서  `First commit: First FT and basic Django config`를 입력하고 저장하면 아래와 같이 커밋 결과를 확인할 수 있다.

```
$ git commit
[master (root-commit) 5b725da] First commit: First FT and basic Django config
 7 files changed, 189 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 function_tests.py
 create mode 100755 manage.py
 create mode 100644 superlists/__init__.py
 create mode 100644 superlists/settings.py
 create mode 100644 superlists/urls.py
 create mode 100644 superlists/wsgi.py

```

# 요약 및 의견

TDD는 예견되는 실패를 만들고 이를 고쳐가는 과정이다.

Django 구동하는 요령은 이미 익숙하므로 Selenium을 구동하는 방법이 가장 중요하다.

git을 로컬 저장소로 사용하는 것을 배우며 github 같은 원격 서비스에 push하는 요령은 다루지 않는다.

출처: [https://wikidocs.net/11057](https://wikidocs.net/11057)