---
layout: default
title: How to use fabric.
---
---------------
# 배포
## Fabrid
- Command-Line Tool
- Streaming the use of SSH for application deployment or systems administration tasks.
- Less codes than shell script
- Less mistakes than handwork
- Less time to operate many servers.

## Basics
- Install
```
$ pip install fabric
```

- Setting(Make fabfile.py)
```
def hello(who="world"):
  print "Hello {who}!".format(who=who)
```

- Run
```
$ fab hello:who=Fab
```

## Running a command on remote hosts
- Setting(Make fabfile.py)
```
from fabric.api import *
env.hosts=["irteam@10.77.0.1", "10.77.0.2"]
env.user="irteam"
env.password="password"
env.parallel=True
def hostname_check():
        run("hostname")
def command(cmd):
        run(cmd)
```

- Run
```
$ fab hostname_check
$ fab command:date
```

## Core Functionality
- local() : Run a command locally
- run() : Run a command remotely
- sudo() : Run a command remotely as another user
- put() : Copy a file from local to remote
- get() : Copy a file from remote to local

## Problems
- Tailing log file (tail -f /var/log/messages) is difficult.

> Fabrid SSH 설정
> >  SSH 키를 사용해서 로그인한다면, SSH 키가 기본 위치에 있고, 서버와 동일 사용자명을 사용하는 한 Fabrid이 정상 동작해야 한다.

## Examples

- [django-fabrid](https://github.com/duointeractive/django-fabtastic/blob/master/examples/fabfile.py)
- https://gist.github.com/gcollazo/495372
