# Companion Object

This article will try to explain companion objects in [Kotlin](https://kotlinlang.org) and also provide examples for 
how they can be accessed via Java. Before companion objects can be explained, it is best to first explain the 
`object` declaration in Kotlin.

## Object

The object declaration in Kotlin is similar to [Scala](https://www.scala-lang.org), it is used to declare a Singleton. 
In the following example a `KotlinObject` is declared, and an assert is used to verify that only one instance exists.

```kotlin
object KotlinObject {
    fun hello() {
        print("hello!")
    } 
}

val firstObject = KotlinObject
val secondObject = KotlinObject

assert(firstObject === secondObject)
```

## Companion Object

A companion object is the declaration of an object inside a class. The companion object can be accessed similarly to 
the way static fields and methods are accessed in Java, the class is used as a qualifier. The following is a declaration
of a companion object in the Person class. 

```kotlin
class Person(val firstName: String, val lastName: String) {
    companion object {
        fun defaultPerson() = Person("Jane", "Smith") 
    }
}

val person = Person.defaultPerson()
```

Since Kotlin is compiled into bytecode, companion objects can be accessed in Java. The following example demonstrates 
the accessing of a companion object in Java. Since there wasn't a name given to the `Person` companion object,
the default name of `Companion` is provided to the object. This means the companion object can be accessed by using the 
qualifier `Person.Companion`.

```java
var person = Person.Companion.defaultPerson();
```

Companion objects are similar to normal objects, so they can implement interfaces. The following example declares a 
`Factory` interface and the `Car` companion object implements the `Factory`. 

```kotlin
interface Factory<T> {
    fun create() : T
}

class Car(val make: String, val model: String) {
    companion object CarFactory: Factory<Car> {
        override fun create(): Car = Car("Honda", "Civic")
    }
}

val car = Car.create()
```

Since the companion object was named as `CarFactory`, the companion object is accessed via `Car.CarFactory` in Java. 

```java
var car = Car.CarFactory.create();
```

Some JVM libraries (e.g., JUnit) depend on static methods. To support these libraries Kotlin has an `@JvmStatic` 
annotation. The `@JvmStatic` annotation instructs the Kotlin compiler to generate real static fields and methods.

```kotlin
class Dog(val breed: String) {
    companion object{
        @JvmStatic
        fun defaultDog() = Dog("mutt")
    }
}
```

In the following Java example, the companion object method can be accessed like a traditional static method because it 
was annotated with `@JvmStatic`.

```java
var dog = Dog.defaultDog();
```

I hope this post helped provide insight into objects and companion objects in Kotlin. If you want to experiment 
with any of the examples, they can be found [here](https://github.com/seanking/companion-objects-kotlin) in GitHub.
