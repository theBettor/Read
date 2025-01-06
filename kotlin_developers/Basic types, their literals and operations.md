# Basic types, their literals and operations

Every language needs a convenient way to represent basic kinds of values, like numbers or characters. All languages need to have built-in types and literals. Types are used to represent certain types of values. Some type examples are Int, Boolean, or String, Literals are built-in notations that are used to create instances. Some literal examples are a string literal, which is test in quotation marks, or an integer literal, which is a bare number.

In this chapter, we’ll learn about the basic Kotlin types and their literals:

- numbers(Int, Long, Double, Float, Short, Byte),
- boolenas(Boolean),
- characters(Char),
- strings(String).

There is also the array primitive type in Kotlin, which will be covered in the chapter Collections.

In Kotlin, all values are considered objects (there are no primitive types), so they all have methods, and their types can be used generic type arguments (this will be covered later). Types that represent numbers, booleans, and characters might be optimized by the Kotlin compiler and used as primitives, but this optimization does not affect Kotlin developers, therefore you don’t need to even think about it.

Let’s start discussing the basic types in Kotlin, one by one.

## Numbers

In Kotlin, there is a range of different types that are used to represent numbers. They can be divied into those representing integer numbers (without decimal points) and those representing floatin-point numbers (with decimal points). In these groups, the difference is in the number of bits used to represent these numbers, which determines the possible number size and precision.

To represent integer numbers, we use Int, Long, Byte, and Short.

To represent floatin-point numbers, we use Float and Double.

A plain number without a decimal point is interpreted as an Int. A plain number with a decimal point is interpreted as a Double.

```kotlin
fun main() {
	val numI = 42
	val numD = 3.14
	num // 이렇게하면 어떤게 Double이고 Int인지 알 수 있음.
```

You can create Long by using the L suffix after the number. Long is also used for number literals that are too big for Int.

```kotlin
fun main() {
	val numL1 = 42L
	val numL2 = 12345678912345
	num // 둘다 롱임을 확인할 수 있음
}
```

Similarly, you can create a Float by ending a number with the F or f suffix.

```kotlin
fun main() {
	val numF1 = 42F
	val numF2 = 123.45F
	num // 둘다 Float임을 확인할 수 있음.
}
```

There is no suffix to create Byte or Short types. However, a number explicitly typed as one of these types will create an instance of this type. This also works for Long.

```kotlin
fun main() {
	val b: Byte = 123
	val s: Short = 345
	val l: Long = 345
}
```

This is not a conversion! Kotlin does not support implicit type conversion, so you cannot use Byte or Long where Int is expected.

```kotlin
fun main() {
	val b: Byte = 123
	val l: Long = 123L
	
	val i1: Int = b
	val i2: Int = l // Type mismatch나고 당연히 Int를 필요로함. 
	// 이런식으로 conversion 할 수 없음.
}
```

If we need to explicitly convert one number to another type, we use explicit conversion functions like toInt or toLong.

```kotlin
fun main() {
	val b: Byte = 123
	val l: Long = 123L
	val i: Int = 123
	
	val i1: Int = b.toInt()
	val i2: Int = l.toInt()
	val l1: Long = b.toLong()
	val l2: Long = i.toLong()
}
```

### Underscores in numbers

In number literals, we can use the underscore _ between digits. This character is ignored, but we sometimes use it to format long numbers for better readability.

```kotlin
fun main() {
	val million = 1_000_000
	println(million) // 1000000
}
```

### Other numeral systems

To define a number using the hexadecimal numeral system, start it with 0x. To define a number using the binary numeral system, start it with 0b. The octal numeral system is not supported.

```kotlin
fun main() {
	val hexBytes = 0xA4_D6_FE_FE
	println(hexBytes) // 2765553406
	val bytes = 0b01010010_01101101_11101000_10010010 
	println(bytes) // 1382934674
}
```

### Number and conversion functions

All basic types that represent numbers are a subtype of the Number type.

```kotlin
fun main() {
	val i: Int = 123
	val b: Byte = 123
	val l: Long = 123L
	
	val n1: Number = i
	val n2: Number = b
	val n3: Number = l
}
```

The Number type specifies transformation functions: from the current number to any other basic type representing a number.

```kotlin
abstract class Number {
	abstract fun toDouble(): Double 
	abstract fun toFloat(): Float 
	abstract fun toLong(): Long 
	abstract fun toInt(): Int 
	abstract fun toChar(): Char 
	abstract fun toShort(): Short 
	abstract fun toByte(): Byte
}
```

This means that for each basic number you can transform it into a different basic number using the to{new type} function. Such functions are known as conversion functions.

```kotlin
fun main() {
	val b: Byte = 123
	val l: Long = b.toLong()
	val f: Float = l.toFloat()
	val i: Int = f.toInt()
	val d: Double = i.toDouble()
	println(d) // 123.0
}
```

### Operations on numbers

Numbers in Kotlin support the basic mathemticla operations:

- addition(+),
- subtraction(-),
- multiplication(*),
- division(/).

```kotlin
fun main() {
	val i1 = 12
	val i2 = 34
	println(i1 + i2) // 46
	println(i1 - i2) // -22
	println(i1 * i2) // 408 
	println(i1 / i2) // 0
	
	val d1 = 1.4
	val d2 = 2.5
	println(d1 + d2) // 3.9
	println(d1 - d2) // -1.1
	println(d1 * d2) // 3.5
	println(d1 / d2) // 0.5599999999999999
}
```

> Notice, that the correct result of 1.4 / 2.5 should be 0.56, not 0.5599999999999999. This problem will be addressed soon.
> 

Beware that when we divide an Int by and Int, the result is also Int, so the decimal part is lost.

```kotlin
fun main() {
	println(5 / 2) // 2, not 2.5
}
```

The solution is first to convert an integer into a floating-point representation and then divide it.

```kotlin
fun main() {
	println(5.toDouble() / 2) // 2.5
}
```

There is also a remainder operator %:

```kotlin
fun main() {
	println(1 % 3) // 1 
	println(2 % 3) // 2 
	println(3 % 3) // 0 
	println(4 % 3) // 1 
	println(5 % 3) // 2 
	println(6 % 3) // 0 
	println(7 % 3) // 1 
	println(0 % 3) // 0 
	println(-1 % 3) // -1 
	println(-2 % 3) // -2 
	println(-3 % 3) // 0
}
```

Kotlin also supports operations that modify a read-write variable var:

- +=,where a += b is the equivalent of a = a + b,
- -=, where a -= b is the equivalent of a = a - b,
- *=, where a *= b is the equivalent of a = a * b,
- /=, where a /= b is the equivalent of a = a / b,
- %=, where a %= b is the equivalent of a = a % b,
- post-incrementation and pre-incrementation ++, which
    
    increment variables value by 1,
    
- post-decrementation and pre-decrementation--, which
    
    decrement variables value by 1.
    

```kotlin
fun main() {
	var i = 1
  println(i) // 1
  i += 10
  println(i) // 11
  i -= 5
  println(i) // 6
  i *= 3
  println(i) // 18
  i /= 2
  println(i) // 9
  i %= 4
  println(i) // 1
  
  // Post-incrementation
  // increments value and returns the previous value
  println(i++) // 1
  println(i) // 2
  // Pre-incrementation
  // increments value and returns the new value
  println(++i) // 3
  println(i) // 3
  // Post-decrementation
  // decrements value and returns the previous value
  println(i--) // 3
  println(i) // 2
  // Pre-decrementation
  // decrements value and returns the new value
  println(--i) // 1
  println(i) // 1
}
```

### Operations on bits

Kotlin also supports operations on bits using the following methods, which can be called using the infix notation (so, between two values):

- and keeps only bits that have 1 in the same binary positions in both numbers.
- or keeps only bits that have 1 in the same binary positions in one or both numbers.
- xor keeps only bits that have exactly one 1 in the same binary positions in both numbers.
- shl shifts the left value left by the right number of bits.
- shr shifts the left value right by the right number of bits, filling the leftmost bits with copies of the sign bit.
- ushr shifts the left value right by the right number of bits, filling the leftmost bits with zeros.

```kotlin
 fun main() {
	 println(0b0101 and 0b0001) // 1, that is 0b0001 
	 println(0b0101 or 0b0001) // 5, that is 0b0101 
	 println(0b0101 xor 0b0001) // 4, that is 0b0100 
	 println(0b0101 shl 1) // 10, that is 0b1010 
	 println(0b0101 shr 1) // 2, that is 0b0010 
	 println(0b0101 ushr 1) // 2, that is 0b0010
 }
```

### BigDecimal and BigInteger

All basic types in Kotlin have limited size and precision, which can lead to imprecise or incorrect results in some situations.

```kotlin
fun main() {
	println(0.1 + 0.2) // 0.30000000000000004
	println(2147483647 + 1) // -2147483648
}
```

This is a standard tradeoff in programming, and in most cases we just need to accept it. However, these are cases where we need to have perfect precision and unlimited number size. On JVM, for unlimited number size we should use BigInteger, which represents a number without a decimal part. For unlimited size and precision, we should use the BigDecimal, which represents a number that has a decimal part. Both can be created using constructors, factory functions (like valueOf), or a conversion from basic types that represent numbers (toBigDecimal and toBigInteger methods).

```kotlin
import java.math.BigDeciaml
import java.math.BigInteger

fun main() {
	val i = 10
	val l = 10L
	val d = 10.0
	val f = 10.0F
	
	val bd1: BigDecimal = BigDecimal(123)
	val bd2: BigDecimal = BigDecimal("123.00") 
	val bd3: BigDecimal = i.toBigDecimal()
	val bd4: BigDecimal = l.toBigDecimal()
	val bd5: BigDecimal = d.toBigDecimal()
	val bd6: BigDecimal = f.toBigDecimal()
	val bi1: BigInteger = BigInteger.valueOf(123) 
	val bi2: BigInteger = BigInteger("123")
	val bi3: BigInteger = i.toBigInteger()
	val bi4: BigInteger = l.toBigInteger()
}
```

BigDecimal and BigInteger also support basic mathematical operators:

```kotlin
import java.math.BigDeciaml
import java.math.BigInteger

fun main() {
	val bd1 = BigDecimal("1.2") 
	val bd2 = BigDecimal("3.4") 
	println(bd1 + bd2) // 4.6 
	println(bd1 - bd2) // -2.2 
	println(bd1 * bd2) // 4.08 
	println(bd1 / bd2) // 0.4
  val bi1 = BigInteger("12") 
  val bi2 = BigInteger("34"). 
  println(bi1 + bi2) // 46. 
  println(bi1 - bi2) // -22. 
  println(bi1 * bi2) // 408. 
  println(bi1 / bi2) // 0
}
```

On platforms other than Kotlin/JVM, external libraries are needed to represent numbers with unlimited size and precision.

### Booleans

Another basic type is Boolean, which has two possible values: true and false.

```kotlin
fun main() {
	val b1: Boolean = true
	println(b1) // true
	val b2: Boolean = false
	println(b2) // false
}
```

We use booleans to express yes/no answers, like:

- Is the user an admin?
- Has the user accepted the cookies policy?
- Are two numbers identical?

In practice, booleans are often a result of some kind of comparison.

### Equality

A Boolean is often a result of equality comparison. In Kotlin, we compare two objects for equality using the double equality sign ==. To check if two objects are not equal, we use the non-equality sign !=.

```kotlin
fun main() {
	println(10 == 10) // true 
	println(10 == 11) // false 
	println(10 != 10) // false 
	println(10 != 11) // true
}
```

Numbers and all objects that are comparable (i.e., they implement the Comparable interface) can also be compared with >, <, >=, and <=.

```kotlin
fun main() {
	println(10 > 10) // false 
	println(10 > 11) // false 
	println(11 > 10) // true
	
  println(10 < 10) // false
  println(10 < 11) // true
  println(11 < 10) // false
  
  println(10 >= 10) // true
  println(10 >= 11) // false
  println(11 >= 10) // true
  
  println(10 <= 10) // true
  println(10 <= 11) // true
  println(11 <= 10) // false
}
```

### Boolean operations

These are three basic logical operators in Kotlin:

- and &&, which returns true when the value on both its sides is true; otherwise, it returns false.
- or ||, which returns true when the values on either of its sides is true: otherwise, it returns false.
- not !, which turns true into false, and false into true.

```kotlin
fun main() {
	println(true && true) // true 
	println(true && false) // false 
	println(false && true) // false 
	println(false && false) // false
	println(true || true) // true 
	println(true || false) // true 
	println(false || true) // true 
	println(false || false) // false
	println(!true) // false
	println(!false) // true
} 
```

Kotlin does not support any kind of automatic conversion to Boolean (or any other type), so logical operators should be used only with objects of type Boolean.

## Characters

To represent a single character, we use the Char type. We specify a character using apostrophes.

```kotlin
fun main() {
	println('A') // A
	println('Z') // Z
}
```

Each character is represented as a Unicode number. To find out the Unicode of a character. use the code property.

```kotlin
fun main() {
	println('A'.code) // 65
}
```

Kotlin accepts Unicode characters. To describe them by their code, we start with \u, and then we need to use hexadecimal format, just like in Java.

```kotlin
fun main() {
	println('\u00A3') // £
}
```

## Strings

Strings are just sequences of characters that form a piece of text. In Kotlin, we create a string using quotation marks “ or triple quotation marks “””.

```kotlin
fun main() {
	val text1 = "ABC"
	println(text1) // ABC
	val text2 = """DEF"""
	println(text2) // DEF
}
```

A string wrapped in single quotation marks requires text in a single line. If we want to define a newline character, we need to use a special character \n. This is no the only thing that needs (or might need) a backslash to be expressed int a string.

![image](https://github.com/user-attachments/assets/2298221a-20fd-4651-ba7e-5401f0808af3)

Strings in triple quotation marks can be multiline; in these strings, special characters can be used directly, and forms prefixed by a backslash don’t work.

```kotlin
fun main() {
	val text1 = "Let\'s say:\n\"Hooray\""
	println(text1)
	// Let's say:
	// "Hooray"
	
	val text2 = """Let\'s say:\n\"Hooray\""""
	println(text2)
	// Let\'s say:\n\"Hooray\"
	
	val text3 = """Let's say:
	"Hooray""""
	println(text3)
	// Lets's say:
	// "Hooray"
}
```

To better format triple quotation mark strings, we use the trimIndent function, which ignores a constant number of spaces for each line.

```kotlin
fun main() {
	val text = """
	Let's say:
	"Hooray"
	""".trimIndent()
	println(text)
	// Let' say:
	// "Hooray"
	
	val description = """
	A
	B
			C
	""".trimIndent()
	println(description)
	// A
	// B
	// 		C
}
```

String literals may contain template expressions, which are pieces of code that are evaluated and whose results are concatenated into a string. A template expression starts with a dolloar sign and consists of either a variable name (like “text is $text”) or an expression is curly braces (like “1 + 2 = ${1 + 2}”).

```kotlin
fun main() {
	val name = "Cookie"
	val surname = "DePies"
	val age = 6
	
	val fullName = "$name $surname ($age)"
	println(fullName) // Cookie DePies (6)
	
	val fullNameUpper = 
	"${name.uppercase()} ${surname.uppercase()} ($age)"
	println(fullNameUpper) // COOKIE DEPIES (6)
	
	val description = """
	Name: $name
	Surname: $surname
	Age: $age
	""".trimIndent()
	println(description)
	// Name: Cookie
	// Surname: DePies
	// Age: 6
}
```

If you need to use a special character inside a triple quotation mark string, the easiest way is to specify it with a regular string and include it using template syntax.

```kotlin
fun main() {
	val text1 = """ABC\nDEF"""
	println(text1) // ABC\nDEF
	val text2 = """ABC${"\n"}DEF"""
	println(text2)
	// ABC
	// DEF
}
```

In Kotlin strings, we use Unicode, so we can also define a Unicode character using a number that starts with \u, and then specifying a Unicode character code in hexadecimal syntax.

![image](https://github.com/user-attachments/assets/2973b564-5ca1-47e7-bc94-68247e57881a)


You can use + operator to concatenate two strings, so to create a new string that is a combination of two other strings. You can also use this operator to concatenate a string with any other object, which will be converted to a string. The result is always a string.

```kotlin
fun main() {
	val text1 = "ABC"
	val text2 = "DEF"
	println(text1 + text2) // ABCDEF
	println(text1 + 123) // ABC123
	println(text1 + true) // ABCtrue
}
```

## Summary

In this chapter, we’ve learned about the basic Kotlin types and the literals we use to create them:

- Numbers that are represented by type Int, Long, Double, Float, Short, and Byte are created with bare number values with possible suffixes for type customization. We can define negative numbers or decimal parts. We can also use underscores for nicer number formatting.
- Boolean values true and false are represented by the Boolean type.
- Characters, which are represented by the Char type. We define a character value using single quotation marks.
- Strings, which are used to represent text, are represented by the String type. Each string is just a series of characters. We define strings inside double quotation marks.

So, we have the foundations for using Kotlin. Let’s move on to more-complicated control structures that determine how our code behaves.

## Exercise: Basic values operations

What will be the result of the following expressions?

```kotlin
fun main() {
    println(1 + 2 * 3) // 7
    println(10 % 3) // 1
    println(-1 % 3) // -1

    println(8.8 / 4) // 2.2
    println(10 / 3) // 3

    println(11.toFloat()) // 11.0
    println(10.10.toInt()) // 10

    var a = 10
    a += 5
    println(a) // 15
    a -= 3
    println(a) // 12
    a++
    println(a) // 13
    println(a++) // 13
    println(a) // 14
    println(--a) // 13
    println(a) // 13

    println(true && false) // false
    println(true || false) // true
    println(!!!!true) // true

    println('A'.code) // 65
    println('A' + 1) // B
    println('C'.code) // 67

    println("A + B") // A + B
    println("A" + "B") // AB
    println("A" + 1) // A1
    println("A" + 1 + 2) // A12
}
```
