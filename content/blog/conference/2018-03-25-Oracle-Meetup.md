---
title: '미래의 Java와 손에 잡히는 Microservice - Oracle Meetup 4th!'
date: 2018-03-25 16:00:00
category: 'conference'
draft: false
---

2018년 3월 17일에 네번째 주최하는 Oracle MeetUp에 참여했다.
3번째 MeetUp에도 참여했었는데 이번에 밋업에 참여한 인원이 훨~씬 많아서
깜짝놀랬다. 


## JAVA 와 OpenJDK

**Java는 개방형이다.**

*java is a blue colour language. It's not PhD thesis material but a language for a job. java feels very familiar to many different programmers because we preferred tried-and-tested things - James Gosling, The Feel of Java*


### 자바 혁신 두가지 흐름

* JCP 이해관계자 중심 개방형 혁신

* 오픈소스 개방형 혁신 

    * 언어의 현대화 : 표현 간소화, 함수형 특성 도입, 다중 코어 시대 대응
    * jvm은 다른 시스템과 통합을 쉽게 할 수 있도록 api를 제공하려고 노력하고 있고,  heap 메모리를 접근할 수 있게 다양하게 제공


----------------------------------

## Open JDK 프로젝트 1) AMBER : 소개 ( 언어를 쉽게 사용할 수 있게끔 )


* JEP 286 **지역변수 타입 추론** ( JAVA10에 적용 )

    * 지속적으로 확대된 타입 추론
    * 자바 5: 제네릭 메서드 타입 추론 Collection.<USER>.method
    * 자바 7: 다이아몬드 연산자
    * 자바 8: 람다식 
    * 자바 10:지역변수 타입 추론 
        ~~~
        var users = VisitorRegister.getVisitors();
        ~~~
        한줄단위로 타입 추론을 하기때문에 필드에는 사용할 수 없음

        java 9부터 null check를 annotation으로 하기때문에 annotation 이 중요한 부분이 되었다. 여기서 var를 쓰면 annotation하기 좋다.

    * var 는 변수 final var 는 상수

    **정적타입 언어**는 타입을 생략하면 타입추론을 통해서 컴파일 시에 타입을 확인해주기때문에 안전하다.


* JEP 301 열거형 개선 X
* JEP 302 람다식 잔반 처리 X 
* JEP 305 패턴 매칭

    * 객체는 값이 아니라 행위가 중요.
    * Switch 식 + 타입 비교 + 객체 분해 + 값 비교


* **JEP 325 switch 식** (**핵심**)

    * Scala는 모든 표현이 식. java는 문과 식이 같이 존재함
    var result = if(조건식) { ... } else { ... } -> 지금까지 java는 switch는 문이었으나 식으로도 사용이 가능

    * switch 식은 패턴 매칭을 연구하다가 나온 부분


* JEP 326 미가공_문자열

    * `` 안에 넣으면 \를 후처리 없는 문자열로 받아들임
    ( 이 부분은 바뀔 가능성이 높음. 문자안에 `가 들어가면 처리하기 다시 힘들기 때문에 )


**AMBER 프로젝트의 목표는 패턴 매칭**


## Open JDK 프로젝트 2) Vahalla ( 발할라 )

* JEP 169 : 값 객체
* JEP 218 : 원시타입 제네릭 ( 제네릭 특화 )
    * JAVA 제네릭 특화는 런타임에 JVM에서 별도의 CLASS를 만들어줌
    * 값 타입에 맞게 동적으로 클래스 변조
* JEP 193 : 변수 핸들 X 

**객체지향의 다형성은 method의 재활용 / 함수형에서 다형성은 제네릭**

## Open JDK 프로젝트 3) 100m (룸) : 화이버

* 컨티뉴에이션 ( Continuation; 지속, 연속 )
    * 작업 단위
    * 중단 후 재실행 가능

* 화이버(Fiber)
    * 쓰레드처럼 동작하지만 쓰레드보다 훨씬 가볍게 동작
    * 컨티뉴에이션을 관리
    * 서버는 대부분 시간을 IO 응답을 기다리지만 쓰레드는 너무 무거움
    * 동기 방식과 유사하게 확장 가능한 프로그래밍 작성 가능
    * 기존 코드를 훨씬 가볍게 돌릴 수 있는 화이버가 필요




## 참고
* [Java.next() AmbEr와 vAlhAllA 프로젝트를 중심으로... 박성철](https://www.slideshare.net/gyumee/javanext?qid=3af9457d-2ecf-45dc-a23a-32571bcfbd15&v=&b=&from_search=1)


* Micro Services - Java the Unix way 필수로 읽어보기


















