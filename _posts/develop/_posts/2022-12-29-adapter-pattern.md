---
layout: post
title: "[디자인 패턴] Adapter 패턴"
subtitle: "[Design Pattern] Adapter Pattern"
description: >
  '헤드퍼스트 디자인패턴' 책을 보며 공부한 내용을 정리한다. 이 중 Adapter 패턴에 대해 알아본다.
categories: develop
sitemap: true
---

&nbsp;Adapter 패턴에 대해 알아본다.


* Table of contents
{:toc}

## Adapter 패턴이란?

&nbsp;Adapter 패턴이란 특정 클래스 인터페이스를 다른 인터페이스로 변환하는 패턴이다. 인터페이스가 호환되지 않아 같이 쓸 수 없는 클래스를 사용할 수 있게 한다.

## Example (Adapter가 필요한 상황)
~~~java
public interface MusicPlayer {
    // 음악을 재생하는 MusicPlayer
    public void playContent();
}

public class MP3Player implements MusicPlayer {
    // MusicPlayer 구상 클래스
    public void playContent() {
        System.out.println("play mp3 music");
    }
}

public interface MoviePlayer {
    // 영상을 재생하는 MoviePlayer
    public void playContent();
}

public class MP4PLayer implements MoviePlayer {
    // MoviePlayer 구상 클래스
    public void playContent() {
        System.out.println("play mp4 movie");
    }
}
~~~
(java 예제. `MusicPlayer` 인터페이스와 구상클래스, `MoviePlayer` 인터페이스와 구상클래스)
{:.figcaption}

&nbsp;위의 예제와 같이 `MusicPlayer`와 `MoviePlayer`가 있는데, `MusicPlayer`가 필요한 곳에 `MoviePlayer`를 대신 사용해야 하는 상황이라고 가정한다. 인터페이스가 다르기 때문에 바로 `MoviePlayer` 구상 클래스를 사용할 수는 없다. 이 때 `MoviePlayer`를 변환하는 Adapter가 필요하게 된다.

## Example (Adapter 클래스)
~~~java
public class PlayerAdapter implements MusicPlayer {
    MoviePlayer player;

    // Adapter의 생성자는 Adaptee 클래스를 인자로 받아 사용한다.
    public PlayerAdapter(MoviePlayer p) {
        this.player = p;
    }

    // Adapter는 Target 인터페이스에 있는 모든 메소드를 구현해야 한다.
    public void playContent() {
        p.playContent();
    }
}
~~~
(java 예제. Adapter 클래스 예제)
{:.figcaption}

&nbsp;Adapter인 `PlayerAdapter` 클래스는 생성자에서 Adaptee인 `MoviePlayer`의 레퍼런스를 인자로 받는다. 이를 통해 Adaptee의 메소드를 호출한다.  
&nbsp;`PlayerAdapter`는 `MusicPlayer` 인터페이스를 사용했기 때문에 사용하는 Client 쪽에서는 이를 `MusicPlayer`로 사용할 수 있다. Adapter안에 실제로 뭐가 들어있는지 알 수도 없고 알 필요도 없다.
