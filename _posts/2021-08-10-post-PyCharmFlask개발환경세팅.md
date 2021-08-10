---
title: "PyCharm/Flask 개발환경 세팅"
categories:
  - Flask
toc: true
tags:
  - Python
  - Flask
  - Setting
last_modified_at: 2021-08-10T22:10:00-05:00
---

# 개요
PyCharm을 이용해서 Flask 개발환경을 어떻게 구성하는지에 대해 조사 한것에 대해 서술


---
# Pycharm 설치
  - https://www.jetbrains.com/ko-kr/pycharm/download/#section=windows
# Python 설치
  - https://www.python.org/downloads/
## Python Interpreter
  - 파이썬은 크게 2, 3버전으로 나누어진다.
  - 2버전과 3버전은 서로 호환이 되지 않기 때문에 각 환경에 맞게 사용해야 한다.
  - File -> New Project을 이용해 프로젝트를 생성시 Base Interpreter를 이용해서 원하는 버전의 Python을 선택 가능

# Flask 세팅
  - File -> Settings -> Proejct -> Python Interpreter -> + (install)를 통해 Flask 설치
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/flask-setting/flask-packages-img.png)

# DB Migrate / Update 세팅
  - File -> Settings -> Proejct -> Python Interpreter -> + (install)를 통해 Flask-Migrate 설치
    - 자동으로 SQLAlchemy가 같이 설치 됨
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/flask-setting/flask-migrate-packages-img.png)
  - 루트 디렉토리에 config.py를 통해 DB 세팅

# Flask Shell 세팅
