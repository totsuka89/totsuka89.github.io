---
layout: post
title: Windows 10 ssh server 활성화하기
subtitle: Activate Windows 10 ssh server
description: >
  Windows 10에서 ssh server 활성화하는 방법을 작성한다.
categories: tools
sitemap: true
---

&nbsp;많은 개발자는 ssh 클라이언트 프로그램, Linux 기반 ssh Server 등을 이용하고 다룰 수 있다. 그렇지만 완전히 동일한 환경으로 Windows 기반 server를 사용한다면 어떨까.
 
&nbsp;Windows PowerShell의 기능이 강력해지면서 Windows server를 Command Line 기반으로 관리하기 용이해졌다. Windows 기반 server에서도 ssh server가 동작한다면 sftp나 scp, VS Code remote 등등 많은 서비스를 Linux server를 관리할 때와 유사하게 이용할 수 있다.

<!--more-->

* Table of contents
{:toc}

## Prerequisite

* `Windows 10 (Version >= 1809)`

## Windows 10 OpenSSH 서버 설치

Windows 10의 `선택적 기능`에서 OpenSSH 서버를 설치한다.

1. 작업 표시줄의 `시작 버튼`을 오른쪽 클릭한 후 `설정`을 클릭한다.

    ![Screenshot_1](/assets/img/blog/2022-11-21/Screenshot_1.png){: width="30%" height="30%" .centered}

2. `설정`에서 `앱`을 선택한다.

    ![Screenshot_2](/assets/img/blog/2022-11-21/Screenshot_2.png){: width="60%" height="60%" .centered}

3. `앱 및 기능` 탭에서 `선택적 기능`을 클릭한다.

    ![Screenshot_3](/assets/img/blog/2022-11-21/Screenshot_3.png){: width="60%" height="60%" .centered}

4. 새로운 기능을 추가하기 위해 `선택적 기능` 항목에서 `기능 추가`를 클릭한다.

    ![Screenshot_4](/assets/img/blog/2022-11-21/Screenshot_4.png){: width="40%" height="40%" .centered}

5. `OpenSSH 서버`를 검색하여 체크박스에 체크한후 설치한다.

    ![Screenshot_5](/assets/img/blog/2022-11-21/Screenshot_5.png){: width="60%" height="60%" .centered}

## Windows 10 OpenSSH 서버 동작

OpenSSH Server가 잘 동작하는지 확인하고 자동실행 되도록 등록한다.

1. 설치가 완료되었으면, 아래와 같은 명령으로 ssh 데몬의 상태를 확인할 수 있다.
```shell
Get-Service sshd
```
    ![Screenshot_6](/assets/img/blog/2022-11-21/Screenshot_6.png){: width="60%" height="60%" .centered}

2. ssh 데몬을 `실행`시키는 명령은 다음과 같다.
```shell
Start-Service sshd
```
    ![Screenshot_7](/assets/img/blog/2022-11-21/Screenshot_7.png){: width="60%" height="60%" .centered}

3. ssh 데몬을 `정지`시키는 명령은 다음과 같다.
```shell
Stop-Service sshd
```
    ![Screenshot_8](/assets/img/blog/2022-11-21/Screenshot_8.png){: width="60%" height="60%" .centered}

4. Windows 부팅 시 ssh 데몬을 자동으로 실행되도록 등록하고 싶다면 다음과 같이 입력한다.
```shell
Set-Service -Name sshd -StartupType 'Automatic'
```
    ![Screenshot_9](/assets/img/blog/2022-11-21/Screenshot_9.png){: width="60%" height="60%" .centered}