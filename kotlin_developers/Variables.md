## Variables

To declare a variable in Kotlin, we start with the val or var keyword, then a variable name, the equality sign, and an initial value.

- The keyword var (which stands for “variable”) represents read-write variables and is used to define variables whose values can be reassigned after initialization. This means that if you use var, you can always assign a new value to this variable.
- The keyword val (which stands for “value”) represents read-only variables and is used to define values that cannot be reassigned. This means that if you use val, you cannot assign a. ew value to this variable once it is initialized.

```kotlin
fun main() {
	val a = 10
	var b = "ABC"
	print(a) // 10
	print(b) // ABC
	// a = 12 is not possible, because a is read-only!
	b = "CDE"
	print(b) // CDE
}
```

We can name variables using characters, underscore _, and numbers (but numbers are not allowed at the first position). By convention, we name variables with the camelCase convention; this means the variable name starts with a lowercase letter, the (instead of using spaces) each next word starts with a capital letter.

Variables don’t need to specify their type explicitly, but this doesn’t mean that variables are not typed. Kotlin is a statically typed language, therefore every variable needs tis type specified. The point is that Kotlin is smart enough to infer the type from the value that is set. 10 is of type Int, so the type of a in the above example is Int. “ABC” is of type String, so the type of b is String.

We can also specify a variable type explicitly using a colon and a type after the variable name.

```kotlin
fun main() {
	val a: Int = 10
	var b: String = "ABC"
	println(a) // 10
	println(b) // ABC
	b = "CDE"
	println(b) // CDE
}
```

When we initialize a variable, we should give it a value. As in the example below, a variable’s definition and initialization can be separated if Kotlin can be sure that the variable won’t be used before any value is set. I suggest avoiding this practice when it’s not necessary.

```kotlin
fun main() {
	val a: Int
	a = 10
	println(a) // 10
}
```

We can assume that a variable should normally be initialized by using an equality sign after its declaration (like in val a = 10). So, what can stand on the right side of the assignment? It can be any expression, i.e., a piece of code that returns a value. Here are the most common types of expressions in Kotlin:

- a basic type literal, like 1 or “ABC”,
- a conditional statement used as an expression, like if-expression, when-expression, or try-catch expression,
- a constructor call,
- a function call,
- an object expression or an object declaration,
- a function literal, like a lambda expression, and anonymous function, or a function reference,
- an element reference.

We have a lot to discuss, so let’s start with basic type literals.

<hr/>

## Kotlin에서 변수 선언은 val 또는 var 키워드를 사용해 시작합니다.

### 변수 선언 키워드
- var (variable): 읽기-쓰기 가능한 변수. 초기화 후 값을 변경할 수 있습니다.
- val (value): 읽기 전용 변수. 초기화 후 값을 변경할 수 없습니다.

### 수 이름 규칙 및 관례
- 변수 이름에는 문자, 밑줄(_), 숫자를 사용할 수 있지만, 숫자는 처음에 올 수 없습니다.
- 카멜케이스(camelCase) 규칙을 따르는 것이 일반적입니다. 예: myVariableName.
### 타입 추론과 명시
- Kotlin은 정적 타입 언어로, 모든 변수는 타입을 가집니다.
- 변수의 타입을 명시하지 않아도, 초기화 값에 따라 타입이 추론됩니다.
  - 예: val a = 10은 Int 타입, var b = "ABC"는 String 타입.
- 타입을 명시하고 싶다면, 변수 이름 뒤에 콜론(:)과 타입을 추가합니다.

### 초기화 규칙
- 변수를 선언할 때 바로 값을 할당하는 것이 일반적입니다.
- 선언과 초기화를 분리할 수도 있지만, Kotlin은 변수가 사용되기 전에 반드시 초기화된다는 것을 보장해야 합니다.

### 할당 표현식
변수 선언 시, 할당 연산자(=)의 오른쪽에는 다양한 표현식이 올 수 있습니다:=
- 기본 타입 리터럴 (예: 1, "ABC")
- 조건문 (예: if-expression, when-expression, try-catch-expression)
- 생성자 호출
- 함수 호출
- 객체 표현식 또는 선언
- 람다 표현식, 익명 함수, 함수 참조 등.

