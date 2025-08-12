# Conditional statements: if, when, try, and while

Most conditional statements, likte the if-condition or the while-loop, look the same in Kotlin, Java, C++, JavaScript, and most other modern languages. For instance, the if-statement is indistinguishable in all theses languages:

```kotlin
if (predicate) {
	// body
}
```

However, the if-condition in Kotlin is more powerful and has capabilities that Kotlin’s predecessors don’t support. I assume that readers of this book have general experience in programming, so I will concentrate on the differences that Kotlin has introduced compared to other programming languages.

### if-statement

Let’s start with the aforementioned if-statment. It executes its body when its condition is satisfied (returns true). We can additionally add the else block, which is executed when the condition is not satisfied (returns false).

```kotlin
fun main() {
	val i = 1
	if (i < 3) { // i < 3 is used as a condition
		// will be executed when condition returns true
		println("Smaller")
	} else {
		// will be executed when condition returns false
		println("Bigger")
	}
	// Prints Smaller if i == 1
}
```

One of Kotlin’s superpowers is that an if-else statement can be used as an expression, therefore it produces a value.

```kotlin
val value = if (condition) {
	// body 1
} else {
	// body 2
}
```

What value is returned? For each body block, it is the result of the last statement (or Unit for an empty body or a statement that is not an expression).

```kotlin
fun main() {
	var isOne = true
	val number1: Int = if (isOne) 1 else 0
	println(number1) // 1
	isOne = false
	val number2: Int = if (isOne) 1 else 0
	println(number2) // 0
	
	val superuser = true
	val hasAccess: Boolean = if (superuser) {
		println("Good morning, sir Admin")
		true
	} else {
			false
	}
	println(hasAccess) // true
}
```

When a body has only statement, its result is the result of our if-else expression. In such a case, we don’t need brackets.

```kotlin
val r: Int = if (one) 1 else 0
// a more readable alternative to
val r: Int = if (one) {
		1
} else {
		0
}
```

This way of using an if-statement is a Kotlin alternative to the Java or JavaScript ternary operator.

```kotlin
// Java
final String name = user == null ? "" : user.name 
// JavaScript
const name = user === null ? "" : user.name
// Kotlin
val name = if (user == null) "" else user.name
```

It should be said that if-else is longer than the ternary operator syntax. I believe this is the main reason why some developers want ternary operator syntax introduced in Kotlin. However, I am against this as if-else is a good replacement that is more readable and can be better formatted. Moreover, we have some additional Kotlin tools, which are also replacements for some ternary-operator use cases: the Elvis operator, extensions on nullable types (like orEmpty), or safe-calls. All these will be explained in detail in the chapter Nullability.

```kotlin
// Java
String name = user == null ? "" : user.name
// Kotlin
val name = user?.name ?: ""
// or
val name = user?.name.orEmpty()
```

Notice that if you use the so-called if-else-if statment, it is just multiple connected if-else statements.

```kotlin
fun main() {
	println("Is it going to rain?")
	val probability = 70
	if (probability < 40) {
			println("Na-ha")
	} else if (probability <= 80) {
			println("Likely")
	} else if (probability < 100) {
			println("Yes")
	} else {
			println("Holly Crab")
	}
}
```

There is actually no such thing as an if-else-if expression: it is just one if-else expression inside another, as can be seen in strange cases where a method is executed on a whole if-else-if expression. Just take a look at the following puzzle and try to predict the result of this code.

```kotlin
// Function we can execute on any object, to print it
// 10.print() prints 10
// "ABC".print() prints ABC

fun Any?.print() {
	print(this)
}

fun printNumberSign(num: Int) {
	if (num < 0) {
			"negative"
	} else if (num > 0) {
			"positive"
	} else {
			"zero"
	}.print()
}

fun main(args: Array<String>) {
	printNumberSign(-2)
	print(",")
	printNumberSign(0)
	print(",")
	printNumberSign(2)
	
	// ,zero,positive

}
```

The answer is not “negative, zero, positive”, because there is no such thing as a single if-else-if expression (just two nested if-else expressions). So. the above printNumberSign implementation gives the same result as the following implementation.

```kotlin
fun printNumberSign(num: Int) {
	if (num < 0){
			"negative"
} else {
		if (num > 0) {
				"positive"
		} else {
				"zero"
		}.print()
	}
}
```

So, when we call print on the result, it is called on the result of the second if-else expression only (the one with “positive” and “zero”). This means that the code above will print “,zero,positive”. How can we fix this? We might use a bracket, but it is generally suggested that, instead of using if-else-if, we should use a when-statement when there is more than one condition. This can helop avoid mistakes like the one in the puzzle above, and it makes code clearer and easier to read.

```kotlin
// Function we can execute on any object, to print it
// 10.print() prints 10
// "ABC".print() prints ABC
fun Any?.print() {
		print(this)
}

fun printNumberSign(num: Int) {
		when {
				num < 0 -> "negative"
				num > 0 -> "posivie"
				else -> "zero"
		}.print()
}

fun main(args: Array<String>) {
		printNumberSign(-2) // negative
		print(",") // ,
		printNumberSign(0) // zero
		print(",") // ,
		printNumberSign(2) // positive
```

### when-statement

The when-statement is an alternative to if-else-if. In every branch, we specify a predicate and the body that should be executed if this predicate returns true(and previous predicates did not). So, it works just like if-else-if but should be preferred because its syntax is better suited for multiple conditions.

```kotlin
fun main() {
		println("Is it going to rain?")
		val probability = 70
		when {
				probability < 40 -> {
						println("Na-ha")
				}
				probability <= 80 -> {
						pritnln("Likely")
				}
				probability < 100 -> }
						println("Yes")
				}
				else -> {
						println("Holly Crab")
				}
		}
}
```

Like in an if-statement, braces are needed only for bodies with more than one statement.

```kotlin
fun main() {
		println("Is it going to rain?")
		val probability = 70
		when {
				probability < 40 -> println("Na-ha")
				probability <= 80 -> println("Likely")
				probability < 100 -> println("Yes")
				else -> println("Holly Crab")
		} 
}
```

The when-statement can also be used as an expression because it can return a value. The result is the last expression of the chosen branch, therefore the following example will print “Likely”

```kotlin
fun main() {
		println("Is it going to rain?")
		val probability = 70
		val text = when {
				probability < 40 -> println("Na-ha")
				probability <= 80 -> println("Likely")
				probability < 100 -> println("Yes")
				else -> println("Holly Crab")
		}
		println(text)
}
```

The when-statement is often used as an expression body:

```kotlin
private fun getEmailErrorId(email: String) = when {
		email.isEmpty() -> R.string.error_field_required
		emailInvalid(email) - > R.string.error_invalid_email
		else -> null
}
```

### when-statement with a value

There is also another form of the when-statement. If we add a value in brackets after the when keyword, then our when-statement becomes an alternative to the switch-case. However, it is a much more powerful alternative because it can not only compare values by equality, but it can also check if an object if of some type (using is), or if an object contains this value (using in). Each block can have multiple values we compare against, separated with a comma.

```kotlin
private val magicNumbers = listOf(7, 13)

fun describe(a: Any?) {
		when (a) {
				null -> println("Nothing")
				1, 2, 3 -> println("Small number")
				in magicNumbers -> println("Magic number")
				in 4..100 -> println("Big number")
				is String -> println("This is just $a")
				is Long, is Int -> println("This is Int or Long")
				else -> println("No idea, really")
		}
}

fun main() {
		describe(null) // nothing
		describe(1) // Small number
		describe(3) // Small number
		describe(7) // Magic number
		describe(9) // Big number.
		// 9는4 4 to 100의 범위에 있기 때문에 Big Number
		describe("AAA") // This is just AAA
		describe(1L) // This is Int or Long
		describe(-1) // This is Int or Long
		describe(1.0) // No idea, really,
		// 1.0은 Double이기 때문에..
}
```

The when-statement with a value can also be used as an expression because it can produce a value:

```kotlin
private val magicNumbers = listOf(7, 13)

fun describe(a: Any?): String = when (a) {
		null -> "Nothing"
		1, 2, 3 -> "Small number"
		in magicNumbers -> "Magic number"
		in 4..100 -> "Big number"
		is String -> "This is just $a"
		is Long, is Int -> "This is Int or Long" 
		else -> "No idea, really"	
}

fun main() {
		println(describe(null)) // Nothing 
		println(describe(1)) // Small number 
		println(describe(3)) // Small number 
		println(describe(7)) // Magic number 
		println(describe(9)) // Big number,
		// because 9 is in range from 4 to 100 
		println(describe("AAA")) // This is just AAA 
		println(describe(1L)) // This is Int or Long 
		println(describe(-1)) // This is Int or Long 
		println(describe(1.0)) // No idea, really, 
		// because 1.0 is Double
}
```

Note that if we use a when-condition as an expression, its conditions must be exhaustive: it should cover all possible branch conditions or provide an else branch, as in the example above. If not all conditions are covered, a compiler error is shown.

Kotlin does not support switch-case statements because we use the when-statement instead.

Inside the “when” parentheses, where we specify a value, we can also define a variable, and its value will be used in each condition.

```kotlin
fun showUser() =
		when (val response = requestUser()) {
				is Success -> showUsers(response.body)
				is HttpError -> showException(response.exception)
		}
```

### is check

Since we have already mentioned the is operator, let’s discuss it in a bit more depth. It checks if a value is of a certain type. We know already that 123 is of type Int, and “ABC” is of type String. Certainly, 123 is not of type String, and “ABC” is not of type Int. We can confirm this using the is check.

```kotlin
fun main() {
		println(123 is Int) // true
		println("ABC" is String) // true
		println(123 is String) // false
		println("ABC" is Int) // false
} 
```

Notice that 123 is an Int, but it is also a Number; the is check returns true for both these types.

```kotlin
fun main() {
		println(123 is Int) // true
		println(123 is Number) // true
		println(3.14 is Double) // true
		println(3.14 is Number) // true
		
		println(123 is Double) // false
		println(3.14 is Int) // false
}
```

When we want to check if a value is not of a certain type, we can use !is; this is an equivalent of is-check, but its result value is negated.

```kotlin
fun main() {
		println(123 !is Int) // false
		println("ABC" !is String) // false
		println(123 !is String) // true
		println("ABC" !is Int) // true
}
```

### Explicit casting

You can always use a value whose type is Int as a Number because every Int is a Number. This process is known as up-casting because we change the value type from lower (more specific) to higher (less specific).

```kotlin
fun main() {
		val i: Int = 123
		val l: Long = 123L
		val d: Dobule = 3.14
		
		var number: Number = i // up-casting from Int to Number
		number = l // up-casting from Long to Number
		number = d // up-casting from Double to Number
}
```

We can implicitly cast from a lower type to a higher one, but not the other way around. Every Int is a Number, but not every Number is an Int because there are more subtypes of Number, including Double or Long. This is why we cannot use Number where Int is expected. However, sometimes we have a situation where we are certain that a value is of a specified type, even though its supertype is used. Explicitly changing from a higher type to a lower type is called down-casting and requires the as operator in Kotlin.

```kotlin
var i: Number = 123

fun main() {
		val j = (i as Int) + 10
		print(j) // 133
}
```

In general, we avoid using as when not necessary because we consider it dangerous. Consider the above example. What if someone changes 123 to 3.14? Both values are of type Number, so the code will compile without any problems or warnings. But 3.14 is Double not Int, and casting is not possible; therefore, the code above will break with a ClassCastException exception.

```kotlin
var i: Number = 3.14

fun main() {
		val j = (i as Int) + 10 // 런타임 에러
		print(j)
}
```

There are two ways to deal with this. The first is to use one of many Kotlin alternatives to cast our value safely. One example is using smart-casting, which will be described in the next section. Another example is a conversion function, like the toInt method, which transforms Number to Int (and possibly loses the decimal part).

```kotlin
var i: Number = 3.14

fun main() {
		val j = i.toInt() + 10
		println(j) // 13
}
```

The second option is the as? operator, which, instead of throwing an exception, reuturns null when casting is not possible. We will discuss handling nullable values later.

```kotlin
var n: Number = 123

fun main() {
		val i: Int? = n as? Int
		println(i) // 123
		val d: Double? = n as? Double
		println(d) // null
}
```

`Number`는 Kotlin의 추상 클래스이며, `Int`, `Double`, `Float`, `Long` 등 숫자 타입의 공통 슈퍼클래스입니다.

### 안전한 캐스팅과 nullable 타입의 결합

- `as?` 연산자는 캐스팅 실패 시 `null`을 반환합니다.
- 이 결과는 nullable 타입(`Int?`, `Double?`)으로 처리해야만 합니다.
- 따라서 `val i: Int?`와 `val d: Double?`로 선언한 것입니다.

In Kotlin, we consider as? a safer option than as, but using both there operators too often is regarded as a code smell. Let’s describe smart-casting, which is their popular alternative.

### Smart-casting

Kotlin has a powerful feature called smart-casting, which allows automatic type casting when the compiler can be sure that a variable if of a certain type. Take a look at the following example:

```kotlin
fun convertToInt(num: Number): Int = 
		if (num is Int) num // the type of num here is Int
		else num.toInt()	
```

The convertToInt function converts an argument of type Number to Int in the following way: if the argument is already of type Int, it is just returned; otherwise, it is converted using the toInt method. Notice that for this code to compile, the num inside the first body needs to be of type Int. In most languages, it needs to be casted, but this happens automatically in Kotlin. Take a look at another example:

```kotlin
fun lengthIfString(a: Any): Int {
		if (a is String) {
				return a.length // the type of a here is String
		}
		return 0
}
```

Inside the if-condition predicate, we checked if a is of type String. The body of this statement will only be executed if the type check is successful. This is why a is of type String inside this body, which is why we can check its length. Such a conversion, from Any to String, is done implicitly by the Kotlin compiler. This can happen only when Kotlin is sure that no other thread can change our property, so when it is either a constant or a local variable. It will not work for non-local var properties because, in such cases, there is no guarantee that they have not been modified between check and usage (e.g., by another thread).

```kotlin
var obj: Any = "AAA"

fun main() {
		if (obj is String) {
				// println(obj.length) will not compile,
        // because `obj` can be modified by some
        // other thread, so Kotlin cannot be sure,
        // that at this point, is it still of type String
		}
}
```

Smart-casting is often used together with when-statements. When they are used together, they are sometimes referred to as ”Kotlin type-safe pattern matching” because they can nicely cover the different possible types a value can be of. More example will ve presented when we discuss the sealed modifier.

```kotlin
fun handleResponse(response: Result<T>) {
		when (response) {
				is Success<*> -> showMessages(response.data)
				// response smart-casted to Success
				is Failure -> showError(response.throwable)
				// response smart-casted to Failure
		}
}
```

- 이 함수는 `Result<T>` 타입의 `response`를 매개변수로 받습니다.
- `Result`는 성공 또는 실패 상태를 나타내는 클래스로, 보통 **sealed class**로 정의됩니다.
- `sealed class`를 사용하면 컴파일러는 `when`이 모든 가능한 하위 클래스를 다루고 있는지 확인할 수 있습니다.
- `Success<*>`에서 `<*>`는 **스타 프로젝션**으로, 성공 데이터의 정확한 타입은 중요하지 않을 때 사용됩니다.
- `when` 블록 내부에서 `response`는 `Success` 타입으로 스마트 캐스팅됩니다.
- 즉, `response`를 `Success` 타입으로 명시적으로 캐스팅하지 않아도, `Success` 클래스의 멤버인 `data`에 바로 접근할 수 있습니다.

1. **명확한 타입 분기**:
    - `sealed class`와 `when`을 사용해 모든 경우를 명확하게 처리할 수 있습니다.
    - 새로운 하위 클래스가 추가되면 컴파일러가 `when` 블록에서 처리되지 않은 경우를 경고합니다.
2. **스마트 캐스팅**:
    - `is` 연산자를 사용하면 명시적인 타입 캐스팅 없이 하위 클래스의 멤버에 접근할 수 있습니다.
3. **가독성**:
    - 코드가 간결하고 읽기 쉽습니다.

### While and do-while statements

The last important control structures we need to mention are while and do-while statements. Both look and work exactly the same as in Java, C++, and many other languages.

```kotlin
fun main() {
		var i = 1
		// while-statement
		while (i < 10) {
				print(i)
				i *= 2
		}
		// 1248
		
		var j = 1
		// do-while statement
		do {
				print(j)
				j *= 2
		} while (j < 10) 
		// 1248
}
```

I hope they don’t need any more explanation. When-statements and do-while-statements cannot be used as expressions. I will only add that both while and do-while statements are rarely used in Kotlin. Instead, we use collection or sequence processing functions, which will be covered in the Functional Kotlin book. For instance, the above code can be replaced with the following:

```kotlin
fun main() {
		generateSequence(1) { it * 2 }
				.takeWhile { it < 10 }
				.forEach(::print)
			// 1248
}
```

### Summary

As you can see, Kotlin has introduced many powerful features to conditional statements. The if-condition and when-condition can be used as expressions. The when-statement is a more powerful alternative to if-else-if or switch-case. Type checks with smart-casting are supproted. All these feature make operating on nullable values both safe and pleasant, which makes the null value our friend, not an enemy. Now, let’s see what Kotlin has changed in functions.

### Exercise: Using when

What will be the result of the following expressions?

```kotlin
private val magicNumbers = listOf(7, 13)

fun name(a: Any?): String = when (a) {
    null -> "Nothing"
    1, 2, 3 -> "Small number"
    in magicNumbers -> "Magic number"
    in 4..100 -> "Big number"
    is String -> "String: $a"
    is Int, is Long -> "Int or Long: $a"
    else -> "No idea, really"
}

fun main() {
    println(name(1)) // Small number
    println(name("A")) // String: A
    println(name(null)) // Nothing
    println(name(5)) // Big number
    println(name(100)) // Big number
    println(name('A')) /////////////////////// No idea, really -> 'A'는 Char 타입으로 문자 하나를 나타내는 타입이다. 그래서 이런 결과를 얻었다.
    println(name("1")) // String: 1
    println(name(-1)) // Int or Long: -1
    println(name(101)) // Int or Long: 101
    println(name(1L)) //////////////////////// Int or Long: 1 -> 1L이 아니라 1로 표시되는 이유 : ${a}러눈 기본 문자열로 인해 1L.toString() 되면서 '1'로 출력됨.
    println(name(7)) // Magic number
    println(name(3)) // Small number
    println(name(3.0)) // No idea, really -> Double 타입에 대한 조건이 while 블록에 없다.
    println(name(100L)) // Int or Long: 100
}
```

### Exercise: Pretty time display

Implement the secondsToPrettyTime function that takes an integer number of seconds and returns a string representation of the time in the following format: “X h Y min Z sec”, where X, Y, and Z are the number of hours, minutes, and seconds respectively. If a value is zero, return “Now”. If the input is negative, return “Invalid input”.

```kotlin
fun secondsToPrettyTime(second: Int): String {
		return ""
}

// Example usage:

println(secondsToPrettyTime(-1)) // Invalid input
println(secondsToPrettyTime(0)) // Now
println(secondsToPrettyTime(45)) // 45 sec
println(secondsToPrettyTime(60)) // 1 min
println(secondsToPrettyTime(150)) // 2 min 30 sec
println(secondsToPrettyTime(1410)) // 23 min 30 sec
println(secondsToPrettyTime(60 * 60)) // 1 h
println(secondsToPrettyTime(3665)) // 1 h 1 min 5 sec
```

# -생략-
