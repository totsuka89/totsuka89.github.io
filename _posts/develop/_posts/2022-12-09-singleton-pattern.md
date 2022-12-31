---
layout: post
title: "[디자인 패턴] Singleton 패턴"
subtitle: "[Design Pattern] Singleton Pattern"
description: >
  '헤드퍼스트 디자인패턴' 책을 보며 공부한 내용을 정리한다. 이 중 Singleton 패턴에 대해 알아본다.
categories: develop
sitemap: true
---

&nbsp;Singleton 패턴에 대해 알아본다.


* Table of contents
{:toc}

## Singleton 패턴이란?

&nbsp;Singleton 패턴이란 말 그대로 특정 객체의 인스턴스를 **하나만** 생성되도록하는 패턴이다. 또한 생성된 객체의 인스턴스를 어디에서든지 참조가 가능해야 한다.  
&nbsp;디자인 패턴의 여러가지 패턴들 중 가장 간단한 패턴 중 하나로 구현하기가 쉽다.

## Example
~~~java
public class Singleton {
    private static Singleton uniqueInstance;

    // any field ...

    private Singleton() {}

    public static Singleton getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }

    // any method ...
}
~~~
(java 예제. 고전적인 Singleton 패턴 구현법)
{:.figcaption}

&nbsp;`uniqueInstance`는 `Singleton` 클래스의 단 하나 생성되는 인스턴스를 저장하는 인스턴스이다.  
&nbsp;생성자를 private로 선언했으므로 `Singleton` 클래스 안에서만 클래스의 인스턴스를 생성할 수 있다. `Singleton` 클래스 내부에서 만들어진 인스턴스를 `getInstance()` 메소드를 통해 리턴받는다.

## 멀티스레딩 문제
&nbsp;객체의 인스턴스를 하나만 유지해야 하지만 멀티스레드 프로그램의 경우 최악의 경우 각 스레드에서 null 체크 블록 안으로 동시에 들어갈 수 있다.

~~~java
public class Singleton {
    // ...
    public static synchronized Singleton getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
    // ...
}
~~~
(solution 1. synchronized 키워드 이용)
{:.figcaption}

&nbsp;`getInstance()` 메소드에 `synchronized` 키워드만 추가하여 한 스레드가 메소드 사용을 끝내기 전까지 다른 스레드를 기다리게 한다. 이 방법은 간단하지만 실질적으로 `getInstance()` 메소드가 꼭 동기화되어야 하는 시점은 `Singleton` 인스턴스가 생성될 때뿐인데 반해 인스턴스가 생성된 이후에도 동기화가 유지되므로 약간의 오버헤드가 발생한다. 프로그램에 큰 부담을 주지 않는다면 사용할 수 있는 방법이다.  

~~~java
public class Singleton {
    private static Singleton uniqueInstance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return uniqueInstance;
    }
    // ...
}
~~~
(solution 2. 처음부터 인스턴스 생성)
{:.figcaption}

&nbsp;static initializer에서 `Singleton`의 인스턴스를 생성한다. 클래스가 로딩될 때 유일한 인스턴스가 생성된다. 미리 만들어서 리턴하는 방식이다. 해당 구문을 지원하는 언어만 사용가능하다는 단점이 있지만 기억해두면 가장 쉽게 사용할 수 있는 방법이다.

&nbsp;이외에도 스레드간 교착상태를 해결할 수 있는 방법이라면 대부분 사용가능하다.