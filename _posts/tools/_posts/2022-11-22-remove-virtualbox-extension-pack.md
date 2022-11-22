---
layout: post
title: VirtualBox Extension Pack 제거하기
subtitle: Uninstalling VirtualBox Extension Pack due to licensing issues
description: >
  라이센스 문제로 인해 VirtualBox Extension Pack을 제거하는 방법을 작성한다.
categories: tools
sitemap: true
---

&nbsp;개발을 하다보면 여러가지 이유로 가상환경을 사용하게 된다. 그 중 대표적으로 VirtualBox, VMware 두 가지 프로그램이 있는데 Docker나 WSL이 점차 강력해짐에도 불구하고 여전히 Legacy환경이나 익숙함을 이유로 계속 사용되고 있다.

![Screenshot_5](/assets/img/blog/2022-11-22/Screenshot_5.png){:.centered}
대표적인 가상환경 프로그램 VirtualBox (좌: VirtualBox 6, 우: VirtualBox 7)
{:.figcaption}

&nbsp;개인 스터디를 위해서 사용하기도 좋으며, 특히 VirtualBox는 기업 및 상업적으로도 무료로 사용할 수 있어 회사에서도 많이 사용하는 툴이기도 하다.

&nbsp;그러나 VirtualBox를 설치하면서 무심코 같이 설치하게 되는 Virtual Extension Pack은 **<u>비상업용 패키지</u>**로서 회사에서는 사용할 수 없다.

![Screenshot_1](/assets/img/blog/2022-11-22/Screenshot_1.png){: width="70%" height="70%" .centered}
문제의 시작
{:.figcaption}

&nbsp;소프트웨어를 설치하면서 대부분 기업용 라이선스 정책을 확인하지 않고 무심코 동의 버튼을 눌러 설치해버리는 경우가 많다. 이러한 사용은 VirtualBox Extension Pack 라이선스 약관에 위배된다.

![Screenshot_2](/assets/img/blog/2022-11-22/Screenshot_2.png){:.centered}
대부분 *~~아주 빠른 속도로~~* 동의 버튼을 눌러버린다.
{:.figcaption}

&nbsp;따라서 회사에서 VirtualBox를 사용하는 사람들은 필히 VirtualBox Extension Pack을 제거하기를 바란다.

## VirtualBox Extension Pack 제거하기

&nbsp;방법은 아주 간단하다.
1. `VirtualBox 6`는 메인 화면의 `환경설정` 버튼을, `VirtualBox 7`는 메인 화면의 `도구` 버튼을 눌러 나오는 서브 메뉴에서 `확장` 버튼을 누른다.

    ![Screenshot_3](/assets/img/blog/2022-11-22/Screenshot_3.png){:.centered}
    (좌: VirtualBox 6, 우: VirtualBox 7)
    {:.figcaption}

2. `VirtualBox 6`는 우측에 작게 위치한, `VirtualBox 7`는 중앙 위쪽에 크게 위치한 `Uninstall` 버튼을 눌러 제거하면 된다.

    ![Screenshot_4](/assets/img/blog/2022-11-22/Screenshot_4.png){:.centered}
    (좌: VirtualBox 6, 우: VirtualBox 7)
    {:.figcaption}