---
title: '특정 데이터가 Serialize 되지 않을 때'
date: 2018-07-30 22:30:00
category: 'web'
draft: false
---

# 특정 데이터가 Serialize 되지 않을 때

**Kotlin 과 JPA로 개발**을 하다가 특정 데이터를 특정 값으로 아무리 insert해도 값이 변하지 않고 초기 값으로 저장되는 것을 확인했다. 문제를 해결하려 계속 이것저것 설정을 바꿔보던 와중에 알게 되었다.

자꾸 값이 저장되지 않고 문제가 생기던 부분

```kotlin
@Entity(name = "member")
@EntityListeners(AuditingEntityListener::class)
class Member {

...
@Column(name = "is_deleted", columnDefinition = "tinyint")
var isDeleted = 0

...

}
```

어떤 부분이 잘못된지 모르고 계속 다른 설정만 변경해보다가 return 해주는 json 데이터에 isDeleted 가 보이지 않는다는 것을 뒤늦게 깨닫고.. Serialize 문제라는 것을 그제서야 알았다.........!

`spring framework, 그리고 kotlin 언어에서` `dto`나 `domain`객체를 이용한 `JSON`의 직렬화/역직렬화(serialize/deserialize)를 할때, **isDeleted, isAccepted, isApproved** 등 `is로 시작하는 column명은 직렬화를 해주지 않아서 계속 값도 변하지 않고 응답 객체에서도 포함이 되지 않았던 것이다..

is 로 시작하는 column명을 직렬화 해주지 않는 부분에 대해서는 kotlin reference에서 찾아 볼 수 있었다.

```
Boolean accessor methods (where the name of the getter starts with is
and the name of the setter starts with set) are represented as properties
which have the same name as the getter method.
```

**kotlin 에서 Boolean 타입은 getter 가 is로 시작되고** setter 는 Java처럼 set 으로 시작되는 부분때문에,
아마 예약어로 받아들여 직렬화가 안됐던 것 같다....

그래서 아래 코드와 같이 @JsonProperty 를 추가해주니 정상적으로 되었다!

```kotlin
@Entity(name = "member")
@EntityListeners(AuditingEntityListener::class)
class Member {

...
@JsonProperty("isDeleted")
@Column(name = "is_deleted", columnDefinition = "tinyint", length = 4)
var isDeleted = 0

...
}
```

단순하게 @JsonProperty를 이용하여 문제를 해결하였는데, 이 외에도 access 설정해주는 부분도 있어서 참고로 링크를 정리해둔다.

[@JsonProperty 를 이용한 접근 제어 - 응답값에 포함하지 않기](http://eglowc.tistory.com/28)

**참고**

[Kotlin Reference](http://kotlinlang.org/docs/reference/java-interop.html)
