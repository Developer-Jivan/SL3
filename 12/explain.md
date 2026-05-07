# What is Scala?

Scala is a modern programming language that combines:

* **Object-Oriented Programming (OOP)** → like Java
* **Functional Programming (FP)** → like Haskell

It runs mainly on the **JVM (Java Virtual Machine)**, so Scala can use Java libraries directly.

---

# Your Code

```scala
object helloWorld{
  def main(args: Array[String]) = {
    println("Hello World!")
  }
}
```

---

# Line-by-Line Explanation

---

## 1. `object helloWorld`

```scala
object helloWorld
```

### Meaning

Creates a **singleton object**.

In Scala:

* `object` = class with only one instance
* Similar to a static class in Java

### Why use `object`?

Scala does not use `static` keyword like Java.

Instead:

* variables/functions that should exist once → placed inside `object`

---

## 2. `{ }`

```scala
{
}
```

Curly braces define the body of the object.

Everything inside belongs to `helloWorld`.

---

## 3. `def main`

```scala
def main(args: Array[String])
```

### `def`

Used to define a function/method.

---

### `main`

Special entry function.

Program execution starts here.

Similar to:

```java
public static void main(String[] args)
```

in Java.

---

### `args: Array[String]`

Command-line arguments.

Example:

```bash
scala hello.scala hi hello
```

Then:

```scala
args(0) = "hi"
args(1) = "hello"
```

---

# ASCII Flow

```text
Program Starts
      ↓
object helloWorld loaded
      ↓
main() executes
      ↓
println() prints text
      ↓
Program Ends
```

---

## 4. `println`

```scala
println("Hello World!")
```

Prints output to console.

### Output

```text
Hello World!
```

---

# Important Scala Concepts

# 1. Object-Oriented Programming (OOP)

Scala supports:

* Classes
* Objects
* Inheritance
* Polymorphism
* Encapsulation

Example:

```scala
class Student(name: String){
  def show() = {
    println(name)
  }
}
```

---

# 2. Functional Programming (FP)

Scala treats functions like values.

Example:

```scala
val square = (x:Int) => x*x

println(square(5))
```

Output:

```text
25
```

---

# Scala vs Java

| Feature                | Scala      | Java     |
| ---------------------- | ---------- | -------- |
| Code Length            | Short      | Longer   |
| Functional Programming | Strong     | Limited  |
| Type Inference         | Yes        | Limited  |
| Immutability           | Encouraged | Optional |
| Syntax                 | Concise    | Verbose  |
| Runs on JVM            | Yes        | Yes      |

---

# Important Scala Syntax

# Variables

---

## Immutable Variable (`val`)

```scala
val name = "Avanish"
```

Cannot change later.

---

## Mutable Variable (`var`)

```scala
var age = 20
age = 21
```

Can change.

---

# Data Types

| Type    | Example |
| ------- | ------- |
| Int     | 10      |
| Double  | 2.5     |
| Float   | 3.2f    |
| String  | "Hello" |
| Boolean | true    |
| Char    | 'A'     |

---

# Functions

```scala
def add(a:Int, b:Int): Int = {
  a + b
}
```

### Explanation

| Part  | Meaning              |
| ----- | -------------------- |
| def   | function definition  |
| a:Int | parameter            |
| : Int | return type          |
| =     | function body starts |

---

# If-Else

```scala
val age = 18

if(age >= 18)
  println("Adult")
else
  println("Minor")
```

---

# Loops

# For Loop

```scala
for(i <- 1 to 5){
  println(i)
}
```

Output:

```text
1
2
3
4
5
```

---

# While Loop

```scala
var i = 1

while(i <= 5){
  println(i)
  i += 1
}
```

---

# Arrays

```scala
val arr = Array(1,2,3)

println(arr(0))
```

Output:

```text
1
```

---

# Lists

Lists are immutable.

```scala
val nums = List(1,2,3,4)
```

---

# Classes and Objects

```scala
class Car{
  def drive() = {
    println("Driving")
  }
}

val c = new Car()
c.drive()
```

---

# Constructor

```scala
class Student(name:String, age:Int){
  def show() = {
    println(name + " " + age)
  }
}
```

---

# Inheritance

```scala
class Animal{
  def sound() = println("Sound")
}

class Dog extends Animal{
  override def sound() = println("Bark")
}
```

---

# Traits (Similar to Interfaces)

```scala
trait Flying{
  def fly()
}
```

Used for abstraction and multiple inheritance.

---

# Collections in Scala

| Collection | Mutable?      |
| ---------- | ------------- |
| List       | No            |
| Array      | Yes           |
| Set        | No duplicates |
| Map        | Key-value     |

---

# Example Map

```scala
val student = Map(
  "name" -> "Avanish",
  "age" -> 20
)
```

---

# Functional Programming Features

# Map

```scala
val nums = List(1,2,3)

val result = nums.map(x => x*2)

println(result)
```

Output:

```text
List(2,4,6)
```

---

# Filter

```scala
nums.filter(x => x%2 == 0)
```

---

# Reduce

```scala
nums.reduce((a,b) => a+b)
```

---

# Anonymous Functions (Lambda)

```scala
(x:Int) => x*x
```

---

# Pattern Matching

Very powerful feature.

```scala
val x = 2

x match{
  case 1 => println("One")
  case 2 => println("Two")
  case _ => println("Other")
}
```

Similar to switch-case but much more powerful.

---

# Companion Object

```scala
class Student{
}

object Student{
}
```

Object with same name as class.

Used for:

* static methods
* factory methods

---

# Case Classes

Used heavily in Scala.

```scala
case class Person(name:String, age:Int)
```

Automatically gives:

* constructor
* equals
* toString
* copy

---

# Collections + FP Style

Scala is famous for combining collections with functional programming.

Example:

```scala
val nums = List(1,2,3,4,5)

val result = nums
  .filter(_ % 2 == 0)
  .map(_ * 10)

println(result)
```

Output:

```text
List(20, 40)
```

---

# Why Scala is Popular

Used in:

* Big Data
* Distributed Systems
* Backend APIs
* Data Engineering

Frameworks:

* Apache Spark
* Akka
* Play Framework

---

# Advantages of Scala

## Pros

* Concise code
* Powerful functional programming
* JVM support
* High performance
* Great for concurrent systems

---

# Disadvantages

## Cons

* Harder syntax initially
* Compilation slower than Java
* Functional concepts can be confusing
* Error messages sometimes complex

---

# Scala Program Structure

```text
Scala Program
│
├── object
│     ├── main()
│     ├── variables
│     └── functions
│
├── classes
├── traits
├── collections
└── functional operations
```

---

# Modern Scala Style

Your code:

```scala
object helloWorld{
  def main(args: Array[String]) = {
    println("Hello World!")
  }
}
```

Modern Scala often uses:

```scala
@main
def hello() =
  println("Hello World!")
```

Cleaner and simpler.

---

# Real Difference Between Java and Scala Thinking

## Java Style

```text
Everything centered around classes
```

## Scala Style

```text
Use immutable data + functions + concise objects
```

---

# Simple Full Example

```scala
object Demo {

  def square(x:Int): Int = {
    x*x
  }

  def main(args:Array[String]) = {

    val nums = List(1,2,3,4)

    val result = nums.map(square)

    println(result)
  }
}
```

Output:

```text
List(1,4,9,16)
```

---

# Interview/Exam Important Points

| Topic             | Key Point                    |
| ----------------- | ---------------------------- |
| object            | Singleton                    |
| val               | Immutable                    |
| var               | Mutable                      |
| def               | Function                     |
| trait             | Interface-like               |
| case class        | Auto-generated methods       |
| map/filter/reduce | Functional operations        |
| JVM               | Runs Scala programs          |
| Scala             | OOP + Functional Programming |
