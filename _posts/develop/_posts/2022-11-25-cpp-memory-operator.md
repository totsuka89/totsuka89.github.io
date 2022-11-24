---
layout: post
title: C++ new, delete 연산자 오버로딩
subtitle: C++ Overloading new, delete operator
description: >
  C++에서 동적 메모리 할당/해제 연산자인 new, delete의 오버로딩에 대해 기술한다.
categories: develop
sitemap: true
---

&nbsp;`new`, `delete`는 동적 할당/해제를 위한 연산자이다.

~~~cpp
void* operator new (std::size_t size);
~~~
heap에 `size`만큼의 메모리 영역을 할당하고 그 주소를 리턴한다.
{:.figcaption}

~~~cpp
void operator delete (void* ptr);
~~~
해당 주소 `ptr`이 가리키는 메모리 영역의 할당을 해제한다.
{:.figcaption}

&nbsp;이 때, 비교적 제한적인 임베디드 환경이나 정적 분석 등을 위하여 C++ 표준 라이브러리의 동적 메모리 할당 기능을 사용하고 싶지 않은 경우가 있을 수 있다.
`new`와 `delete`는 단순한 `keyword`가 아닌 `연산자(operator)`이므로, 연산자 오버로딩이 가능하다.

<!--more-->

* Table of contents
{:toc}

## Outline

&nbsp;먼저 `new`와 `delete`가 정확히 하는 일을 알아보자.

* new

&nbsp;메모리 할당 -> 생성자 호출 -> 메모리 주소 타입 캐스팅

`new`는 `malloc`과는 다르게 단순히 메모리 할당만이 아닌 생성자를 호출한다는 점이 가장 중요하다.

* delete

&nbsp;소멸자 호출 -> 메모리 해제

&nbsp;`delete`는 `free`와는 다르게 단순히 메모리 해제만이 아닌 소멸자를 호출한다는 점이 가장 중요하다.

## Overloading

* new

&nbsp;`new` 연산자는 위에 작성한 연산자의 정의에 맞게 `void*`를 반환하고, 인자는 `size_t`로 받기로 약속되어있다. 이는 자동으로 오버로딩한 클래스의 생성자를 호출해주며, 메모리 주소의 타입캐스팅을 알아서 해준다는 말이다.

~~~cpp
class MyClass {
    // any constructor
public:
    void* operator new (size_t size) {

        // something
        
        void* adr = MyAlloc(size);

        // something

        return adr;
    }
};
~~~

&nbsp;위와 같은 클래스이 정의되었다면 아래와 같은 구문을 만났을 때, 생성자도 호출이 되며 메모리 할당도 표준 메모리 함수가 아닌 `MyAlloc()`가 할 것이다.

~~~cpp
MyClass* c = new MyClass();
~~~

* delete

&nbsp;`delete` 연산자는 `new`에 비해 더욱 간단하다. 위에 작성한 연산자의 정의에 따라 인자로 받은 `ptr`이 가리키는 메모리를 해제해 주기만 하면된다. 이는 자동으로 소멸자를 호출한다는 뜻이기도 하다.

~~~cpp
class MyClass {
    // any destructor
public:
    void operator delete (void* ptr) {

        // something

        MyFree(ptr);

        // something
    }
};
~~~

&nbsp;위와 같이 클래스가 정의되었다면 아래와 같은 구문을 만났을 때, 소멸자도 호출이 되며 메모리 해제도 표준 메모리 함수가 아닌 `MyFree()`가 할 것이다.

~~~cpp
MyClass* c = new MyClass();
delete c;
~~~

## 배열 형태의 new, delete

&nbsp;위에서 보인 예시와 같이 `new`, `delete`를 오버로딩 할 수 있다. 또한 배열의 형태로 `new`, `delete`가 호출이 될 때도 있다. 요약하자면 다음과 같은 형태로 오버로딩이 가능하다.

~~~cpp
class MyClass {
public:
    // 객체의 할당/해제
    void* operator new (size_t size) { ... }
    void  operator delete (void* ptr) { ... }

    // 객체 배열의 할당/해제
    void* operator new[] (size_t size) { ... }
    void  operator delete[] (void* ptr) { ... }
};
~~~

&nbsp;위 클래스에서 오버로딩된 `new`, `delete`는 각각 아래의 문장에서 호출된다.

~~~cpp
MyClass* c = new MyClass();
delete c;

MyClass* arr = new MyClass[5];
delete []arr;
~~~

## new, delete의 호출

&nbsp;`new`와 `delete`가 클래스내에 멤버함수의 형태로 오버로딩 되어있음에도 불구하고 클래스가 생성되기 전에 호출이 가능한 것은 그들이 `public` 속성을 가지고 `static` 함수로 간주가 되기 때문이다.