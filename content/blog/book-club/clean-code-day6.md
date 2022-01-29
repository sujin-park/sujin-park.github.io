---
title: 'CleanCode(클린코드) - 3장. 함수'
date: 2022-01-26 22:00:00
category: 'book-club'
draft: false
---

🔖 오늘 읽은 범위 : 3장 함수

---

😃 **책에서 기억하고 싶은 내용을 써보세요.**


**작게 만들어라!**

- 함수를 만드는 첫째 규칙은 작게!다. 함수를 만드는 둘째 규칙은 더 작게!다. (42p)
- 각 함수가 너무도 명백했다. 각 함수가 이야기 하나를 표현했다. (43p)
- 블록과 들여쓰기
    - If 문/else 문/while 문 등에 들어가는 블록은 한 줄이어야 한다.
    - 대개 거기서 함수를 호출한다. 중첩 구조가 생길만큼 함수가 커져서는 안 된다는 뜻이다.
    - 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안 된다.

**한가지만 해라!**

- 지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 한다.
- 단순히 다른 표현이 아니라 의미 있는 이름으로 다른 함수를 추출할 수 있다면 그 함수는 여러 작업을 하는 셈이다. (45p)
- 함수 내 섹션
    - 한 가지 작업만 하는 함수는 자연스럽게 섹션으로 나누기 어렵다. (45p)

**함수 당 추상화 수준은 하나로!**

- 함수가 한 가지 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일해야 한다. (45p)
- 한 함수 내에 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다.
    - `getHtml()` 은 추상화 수준이 아주 높다. `render` 는 추상화 수준이 중간이다. `append(‘\n’)`와 같은 코드는 추상화 수준이 아주 낮다.
- 위에서 아래로 코드 읽기: 내려가기 규칙
    - 위에서 아래로 프로그램을 읽으면 함수 추상화 수준이 한 번에 한 단계씩 낮아진다.
    - 코드는 위에서 아래로 이야기처럼 읽혀야 좋다. (46p)

**Switch**

- 다형적 객체를 생성하는 코드 안에서는 스위치 문을 참는다. 이렇게 상속 관계로 숨긴 후에는 절대로 다른 코드에 노출하지 않는다. (49p)

**서술적인 이름을 사용하라! (50p)**

- 서술적인 이름을 사용하면 개발자 머릿속에서도 설계가 뚜렷해지므로 코드를 개선하기 쉬워진다.
- 이름을 붙일 때는 일관성이 있어야 한다. 모듈 내에서 함수 이름은 같은 문구, 명사, 동사를 사용한다.

**함수 인수**

- 함수에서 이상적인 인수 개수는 0개이다.
- 테스트 관점에서 보면 인수는 더 어렵다. 갖가지 인수 조합으로 함수를 검증하는 테스트 케이스를 작성한다고 상상해보라!
- SetupTeardownIncluder.render(pageData) 는 이해하기 아주 쉽다. `pageData`객체 내용을 렌더링하겠다는 뜻이다.
- 많이 쓰는 단항 형식
    - 함수에 인수 1개를 넘기는 이유로 가장 흔한 경우는 두 가지다.
    - 하나는 인수에 질문을 던지는 경우다. `boolean fileExists(‘MyFile’)` 이 좋은 예다. 다른 하나는 인수를 뭔가로 변환해 결과를 변환하는 경우다. InputStream fileOpen(‘MyFile’) 은 String 형의 파일 이름을 InputStream 으로 변환한다.
- 플래그 인수
    - 함수로 boolean 값을 넘기는 관례는 정말 끔찍하다. 플래그가 참이면 이걸 하고 거짓이면 저걸 한다는 말이니까! (52p)
- 이항 함수
    - 인수가 2개인 함수는 인수가 1개인 함수보다 이해하기 어렵다.
    - Point p = new Point(0, 0) 가 좋은 예다.
        - 인수 2개는 한 값을 표현하는 두 요소다. 직교 좌표계 점은 일반적으로 인수 2개를 취한다. (52p)
    - `assertEquals(expected, actual)` 은 당연하게 여겨지는 이항 함수인데, expected 와 actual 의 순서를 인위적으로 기억해야 한다. 여기에도 문제가 있다. (52p)
- 삼항 함수
    - 인수가 3개인 함수는 인수가 2개인 함수보다 훨씬 더 이해하기 어렵다.
- 인수 객체
    - 인수가 2-3개 필요하다면 일부를 독자적인 클래스 변수로 선언할 가능성을 짚어본다.
    - 변수를 묶어 넘기려면 이름을 붙여야 하므로 결국은 개념을 표현하게 된다.
- 동사와 키워드
    - 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야 한다.
        - 예시는 write(name) 은 누구나 곧바로 이해한다. 여기를 writeField(name) 으로 하면 이름이 필드라는 사실이 분명히 드러난다. (54p)
    - 함수 이름에 인수 이름을 넣는다.
        - 예시는 assertEquals 보다는 assertExpectedEqualsActual(expected, actual) 로 하면 인수 순서를 기억할 필요가 없다.
    

**부수 효과를 일으키지 마라!**

아래는 표준 알고리즘을 사용해 userName 과 password 를 확인한다. 두 인수가 올바르면 true 를 반환하고 아니면 false 를 반환한다. 하지만 함수는 부수 효과를 일으킨다.

```java
public class UserValidator {
	private Cryptographer cryptographer;

	public boolean checkPassword(String name, String password) { 
		User user = UserGateway.findByName(userName);
		if (user != User.NULL) { 
			String codedPhrase = user.getPhraseEncodedByPassword();
			String phrase = cryptographer.decrypt(codedPhrase, password);
			if ("Valid Password".equals(phrase)) { 
				Session.initialize();
				return true;
			}
		}
		return false;
	}
}
```

위 코드에서 함수가 일으키는 부수 효과는 Session.initialize() 호출이다. 이름만 봐서는 세션을 초기화한다는 사실이 드러나지 않는다. 그래서 함수 이름만 보고 함수를 호출하는 사용자는 사용자를 인증하면서 기존 세션 정보를 지워버릴 위험에 처한다.

이런 부수 효과가 시간적인 결합을 초래한다. 이런 시간적인 결합이 필요하다면 함수명을 바꾼다. checkPasswordAndInitializeSession 이라는 이름이 훨씬 좋다.

**명령과 조회를 분리하라!**

- 함수는 뭔가를 수행하거나 뭔가에 답하거나 둘 중 하나만 해야 한다.
- 객체 상태를 변경하거나 아니면 객체 정보를 반환하거나 둘 중 하나다.
    
    ```java
    public boolean set(String attribute, String value);
    
    if (set('username', 'unclebob'))...
    ```
    
    이 함수는 이름이 attribute 인 속성을 찾아 값을 value 로 설정한 후 성공하면 true 를 반환하고 실패하면 false 를 반환한다. 함수를 호출하는 코드만 봐서는 set 이 설정되어 있는지 확인하는 코드인지, 설정하는 코드인지 분간하기 어렵다.
    
    진짜 해결책은 명령과 조회를 분리해 혼란을 애초에 뿌리뽑는 방법이다.
    
    ```java
    if (attributeExists('username') {
    	setAttribute('username', 'unclebob');
    	...
    }
    ```
    

**오류코드보다 예외를 사용하라!**

- 명령 함수에서 오류 코드를 반환하는 방식은 명령/조회 분리 규칙을 미묘하게 위반한다.
- 자칫하면 if 문에서 명령을 표현식으로 사용하기 쉬운 탓이다.

```java
if (deletePage(page) === E_OK)
```

위 코드는 동사/형용사 혼란을 일으키지 않는 대신 여러 단계로 중첩되는 코드를 야기한다. 오류 코드를 반환하면 호출자는 오류 코드를 곧바로 처리해야 한다는 문제에 부딪힌다.

오류 코드 대신 예외를 사용하면 오류 처리 코드가 원래 코드에서 분리되므로 코드가 깔끔해진다.

```java
try {
	deletePage(page);
	registry.deleteReference(page.name);
	configKeys.deleteKey(page.name.makeKey());
} catch (Exception e) {
	logger.log(e.getMessage());
}
```

- Try/Catch 블록 뽑아내기

```java
try {
	deletePageAndAllReferences();
} catch (Exception e) {
	logError(e);
}

private void deletePageAndAllReferences(Page page) throws Exception {
	deletePage(page);
	registry.deleteReference(page.name);
	configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e) {
	logger.log(e.getMessage());
}
```

- 오류 처리도 한 가지 작업이다.
    - 오류를 처리하는 함수는 오류만 처리해야 마땅하다. 위 예제처럼
    - 함수에 키워드 try 가 있다면 함수는 try 문으로 시작해 catch/finally 문으로 끝나야 한다는 말이다.

**반복하지 마라!**

**구조적 프로그래밍**

- 함수는 return 문이 하나여야 한다.
- 루프 안에서 break 나 continue 를 사용해선 안 되며 goto 는 절대로, 절대로 안된다.
- 함수를 작게 만든다면 간혹 return, break, continue 를 여러 차례 사용해도 괜찮다.

**함수를 어떻게 짜죠?**

- 함수를 짤 때는 처음에는 길고 복잡하다. 들여쓰기 단계도 많고 중복된 루프도 많다. 코드를 다듬고, 함수를 만들고, 이름을 바꾸고, 중복을 제거한다. 메서드를 줄이고 순서를 바꾼다. 이와중에도 코드는 항상 단위 테스트를 통과한다.

<br>

🤔 **오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요**

시스템이라는 이야기를 풀어가는데 있다는 사실을 명심하자.

작성하는 함수가 분명하고 정확한 언어로 깔끔하게 같이 맞아떨어져야 이야기를 풀어가기가 쉬워진다는 사실을 기억하자.

<br>

🔥 **소감 3줄 요약**

- 함수를 만드는 첫째 규칙은 작게!다. 함수를 만드는 둘째 규칙은 더 작게!다.
- 함수에서 이상적인 인수 개수는 0개이다.
- 지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 한다.