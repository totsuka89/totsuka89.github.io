---
layout: post
title: Repo 사용하기 1 (설치)
subtitle: Repo guide 1 (예제)
description: >
  Git Repo를 이용하여 여러 git repository를 한번에 관리하는 방법을 기술한다.
  이 중 가장 먼저 repo를 설치하는 방법과 발생할 수 있는 에러에 대해 살펴본다.
categories: tools
sitemap: true
---

&nbsp;repo는 원래 Android 프로젝트에서 사용되던 실행 가능한 python 스크립트이다. Android라는 프로젝트가 워낙 덩치가 큰 프로젝트인데다가 모듈화된 소스코드는 각각의 Git Repository에 저장되어 있기 때문에 이를 통합하여 관리하기 위한 툴이 필요했다.

&nbsp;비단 Android 뿐만이 아니라 자신의 프로젝트가 여러개의 Git Repository와 Submodule로 이루어져 있다면 repo를 적용해보는 것도 좋을 것이다.

* Table of contents
{:toc}

## Install
&nbsp;repo 프로젝트의 문서[^1]를 참고하여 repo를 설치한다.

&nbsp;각 OS 패키지 저장소에서 설치가 가능한 경우는 패키지 저장소로부터 설치한다.
~~~shell
# For Debian/Ubuntu.
$ sudo apt-get install repo

# For Gentoo.
$ sudo emerge dev-vcs/repo
~~~
&nbsp;또한 패키지 저장소를 거치지 않고 다음과 같이 command line에서 설치할 수도 있다.
~~~shell
$ mkdir -p ~/.bin
$ PATH="${HOME}/.bin:${PATH}"
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
$ chmod a+rx ~/.bin/repo
~~~

&nbsp;설치가 이상없이 되었다면 command line에서 확인이 가능하다.
~~~shell
$ repo --version
repo version v2.30
       (from https://gerrit.googlesource.com/git-repo)
       (tracking refs/heads/stable)
       (Wed, 16 Nov 2022 18:26:49 +0000)
repo launcher version 2.29
       (from /usr/bin/repo)
       (currently at 2.30)
repo User-Agent git-repo/2.30 (Linux) git/2.34.1 Python/3.10.6
~~~

## Exception
&nbsp;curl 명령을 통해 manual 설치를 한다면 상관없지만 OS 패키지 저장소로부터 repo를 설치했다면 이후 repo를 사용할 때, 최신 버전이 아니라는 메시지를 출력할 수도 있다.
~~~
... A new version of repo (2.29) is available.
... New version is available at: /home/user/temp/.repo/repo/repo
... The launcher is run from: /usr/bin/repo
!!! The launcher is not writable.  Please talk to your sysadmin or distro
!!! to get an update installed.
~~~
&nbsp;이럴땐 메시지에서 알려주는 위치에 있는 최신버전의 repo를 기존 repo의 위치로 복사하여 주면 된다.
~~~
cp /home/user/temp/.repo/repo/repo /usr/bin/repo
~~~
&nbsp;한 번에 해결된다면 좋겠지만, 최신버전의 repo로 교체하면 다음과 같은 추가적인 에러가 발생할 수 있다.
~~~
/usr/bin/env: ‘python’: No such file or directory
~~~
&nbsp;기본적으로 repo는 python 스크립트이기 때문에 버전에 맞는 python를 참조하여야 한다.
&nbsp;python3를 참조하는 다른 곳에서도 날 수 있는 에러로 해결하는 법은 간단하다. 본 포스트에서는 ubuntu 기준으로 설명하고자 한다.
1. python3가 설치되어 있지 않다면 설치해 준다.
    ~~~
    apt install python3
    ~~~
2. 만약 python3가 설치되어 있음에도 다음과 같은 문제가 발생한다면 python3의 커맨드가 python이 아니라 python3일 가능성이 높다. 다음과 같이 심볼릭 링크로 연결해준다.
    ~~~
    ln -s /usr/bin/python3 /usr/bin/python
    ~~~
3. ubuntu에서의 또다른 방법으로는 python-is-python3 패키지를 설치하는 방법이 있다. python-is-python3 패키지를 설치하면 /usr/bin 안에 python3를 가리키는 python링크 파일이 생성된 것을 확인할 수 있다. 2번의 방법과 같은 효과임을 알 수 있다.
    ~~~
    apt install python-is-python3
    ~~~


[^1]:[https://gerrit.googlesource.com/git-repo/+/HEAD/README.md](https://gerrit.googlesource.com/git-repo/+/HEAD/README.md)
