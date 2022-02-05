---
title: 'CleanCode(클린코드) - 7장. 오류 처리'
date: 2022-02-01 20:00:00
category: 'book-club'
draft: false
---

🔖 오늘 읽은 범위 : 7장 - 오류 처리 (day13, 15)

---

<br>

## 😃 **책에서 기억하고 싶은 내용을 써보세요.**


### **오류 코드보다 예외를 사용하라**

- 오류가 발생하면 예외를 던지는 편이 낫다. 그러면 호출자 코드가 더 깔끔해진다. 논리가 오류 처리 코드와 뒤섞이지 않으니까. (p.131)

```java
public class DeviceController {
	...

	public void sendShutDown() {
		try {
			tryToShutDown();
		} catch (DeviceShutDownError e) { 
			logger.log(e);
		}
	}

	private void tryToShutDown() throws DeviceShutDownError { 
		getHandle();
	}

	private DeviceHandle getHandle() { 
		...
		throw new DeviceShutDownError('invalid .... ');
	}

}
```

### **Try-Catch-Finally 문부터 작성하라**

- 예외가 발생할 코드를 짤 때는 try-catch-finally 문으로 시작하는 편이 낫다. 그러면 try 블록에서 무슨 일이 생기든지 호출자가 기대하는 상태를 정의하기 쉬워진다.

### **예외에 의미를 제공하라**

- 오류 메시지에 정보를 담아 예외와 함께 던진다. 실패한 연산 이름과 실패 유형도 언급한다. (p.135)
- 예외를 던질 때는 전후 상황을 충분히 덧붙인다.

### **호출자를 고려해 예외 클래스를 정의하라**

- 애플리케이션에서 오류를 정의할 때 프로그래머에게 가장 중요한 관심사는 오류를 잡아내는 방법이 되어야 한다.
- 외부 API 를 사용할 때는 감싸기 비법이 최선이다. 외부 API 를 감싸면 외부 라이브러리와 프로그램 사이에서 의존성이 크게 줄어든다.

### **정상 흐름을 정의하라**

- 외부 API 를 감싸 독자적인 예외를 던지고, 코드 위에 처리기를 정의해 중단된 계산을 처리하는 방식이 대개는 멋지지만, 때로는 중단이 적합하지 않은 때도 있다.

아래는 비용 청구 애플리케이션에서 총계를 계산하는 허술한 코드다. 예외가 논리를 따라가기 어렵게 만든다. 특수 상황을 처리할 필요가 없다면 코드가 더 간결해질 것이다.

```java
try { 
	MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
	m_total += expenses.getTotal();
} catch (MealExpensesNotFound e) {
	m_total += getMealPerDiem();
}
```

```java
public class PerDiemExpenses implements MealExpenses {
	public int getTotal() { 
		// 기본값으로 일일 기본 식비를 반환한다.
	}
}

MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();
```

ExpenseReportDAO 를 고쳐 위와 같이 청구한 식비가 없다면 일일 기본 식비를 반환하는 MealExpenses 객체를 반환하면 된다. 이를 `특수 사례 패턴` 이라고 부른다. 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식이다.

### **null 을 반환하지 마라**

- 한 줄 건너 하나씩 null 을 확인하는 코드로 가득한 애플리케이션을 지금까지 수도 없이 봤다.
- null 을 반환하지 않고 빈 리스트를 반환한다면 코드가 훨씬 깔끔해진다.

### **null 을 전달하지 마라**

- 메서드에서 null 을 반환하는 방식도 나쁘지만 메서드로 null 을 전달하는 방식은 더 나쁘다.
- 누군가 null 을 전달하면 실행 오류가 발생한다.

### **결론**

- 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다.
- 오류 처리를 프로그램 논리와 분리해 독자적인 사안으로 고려하면 튼튼하고 깨끗한 코드를 작성할 수 있다.
- 오류 처리를 프로그램 논리와 분리하면 독립적인 추론이 가능해지며 코드 유지보수성도 크게 높아진다.

<br>

## 🤔 **오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요**


- 오류 코드 또는 if 문으로 작성하는 것보다 예외 처리를 하는 것이 더 좋다는 것을 알고 있음에도 불구하고, 무의식 중에 오류 코드나 if 문을 사용하게 된다. 이번 7장을 읽고 예외 처리를 하는 것을 한번 더 각인시킬 수 있었다.
- 안정성도 높으면서 깨끗한 코드를 작성할 수 있게 노력해야겠다.

<br>

## 🔥 **소감 3줄 요약**


- 오류 처리를 프로그램 논리와 분리해 독자적인 사안으로 고려하면 튼튼하고 깨끗한 코드를 작성할 수 있다.
- 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다. **명심!**
- Javascript 에는 NullPointerException 이 없고 optional chaining 을 사용하면서 메서드에서 null 이 반환되는 경우를 종종 보았는데, 이런 부분들을 발견하면 리팩토링 해봐야겠다.