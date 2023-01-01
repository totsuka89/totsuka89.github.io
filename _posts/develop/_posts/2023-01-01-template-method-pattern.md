---
layout: post
title: "[디자인 패턴] Template Method 패턴"
subtitle: "[Design Pattern] Template Method Pattern"
description: >
  '헤드퍼스트 디자인패턴' 책을 보며 공부한 내용을 정리한다. 이 중 Template Method 패턴에 대해 알아본다.
categories: develop
sitemap: true
---

&nbsp;Template Method 패턴에 대해 알아본다.


* Table of contents
{:toc}

## Template Method 패턴이란?

&nbsp;Template Method 패턴은 알고리즘의 골격을 정의하는 패턴이다. 어떤 특정한 알고리즘을 서브클래스와 함께 수행할 때, 알고리즘의 일부 단계를 서브클래스에서 구현할 수 있다. 핵심은 알고리즘의 구조는 그대로 유지하면서 원하는 단계만을 서브클래스에서 구현하는 것이다.

## Example
~~~java
abstract class AbstractClass {
    final void templateMethod() {
        primitiveOperation1();
        primitiveOperation2();
        concreteOperation();
    }

    abstract void primitiveOperation1();
    abstract void primitiveOperation2();

    void concreteOperation() {
        System.out.println("AbstractClass's concreteOperation");
    }
}

public class SubClass1 {
    public void primitiveOperation1() {
        System.out.println("SubClass1's primitiveOperation1");
    }
    public void primitiveOperation2() {
        System.out.println("SubClass1's primitiveOperation2");
    }
}

public class SubClass2 {
    public void primitiveOperation1() {
        System.out.println("SubClass2's primitiveOperation1");
    }
    public void primitiveOperation2() {
        System.out.println("SubClass2's primitiveOperation2");
    }
}
~~~
(java 예제. Template Method 패턴 예제)
{:.figcaption}

&nbsp;먼저 `AbstractClass`의 `templateMethod()`는 알고리즘의 템플릿이 된다. `AbstractClass`의 서브클래스에서 알고리즘의 각 단계를 수정하지 못하도록 final로 선언한다.  
&nbsp;`primitiveOperation1()`, `primitiveOperation2()` 두 메소드는 알고리즘의 각 단계를 나타내는 메소드이며, `AbstractClass`의 서브클래스에서 구현한다. 알고리즘 내부에 서브클래스마다 다른 행동이 아니라 공통적으로 수행해야하는 단계가 있다면 `concreteOperation()`와 같이 추상 클래스 안에서 정의되는 메소드도 있다.  
  
&nbsp;Example에서 보다시피 각 서브클래스인 `SubClass1`, `SubClass2`안에서는 각각의 `primitiveOperation1()`, `primitiveOperation2()`의 구현을 통해 알고리즘의 특정 단계를 제공하고 있다.
