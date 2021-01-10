---
layout: post
title: Scoped Functions in Kotlin
description: Scoped Functions in Kotlin
---
# Purpose

While I have always be interested in Kotlin, I now starting to use it more professionally. Like most languages, there are many things that I like and a few things that I don't like about Kotlin. Over the next few blog posts, I plan to describe some of the features with in the language that I find benefical and some that are confusing and/or annoying.  

# Scope Functions

Scope functions have been a pleasant surprise. Scope functions in Kotlin provide the ability to execute a block of code within the context of an object. This does sound trivial, but let me provide some examples that I hope will clariry their capabilities. 

There are five scope functions: `let`, `run`, `with`, `apply`, and `also`. Each function is slightly different in object reference and return value. The table below provides a overview of the differences between the functions. Next, I will go over each of these functions in more detail.

### Function Selection 

|Function	| Object reference | Return value   | Is extension function
|-----------|------------------|----------------|----------------------
| let       | it               | Lambda result  | Yes 
| run       | this             | Lambda result  | Yes
| run       | -                | Lambda result  | No: called without the context object
| with      | this             | Context object | No: takes the context object as an argument
| apply     | this             | Context object | Yes
| also      | it               | Context object | Yes

The previous table was copied from Kotlin's website. See: [function-selection](https://kotlinlang.org/docs/reference/scope-functions.html#function-selection)

## Let Function

The let function is heavily used within Kotlin. It is often used for executing lambdas on non-null objects and for introducing an expression as a variable in local scope. 


```kotlin
@Test
fun `should use _it_ as object reference and return lambda result using _let_ function`() {
    // Given
    val numbers = mutableListOf<Int>()

    // When
    val count = numbers.let {
        it.add(1)
        it.add(2)
        it.count()
    }

    // Then
    assertThat(count).isEqualTo(2)
}
```

The previous example executes the `let` function on a list of integers. The lambda then intrements the count the list using the `it` variable and returns the resulting count. This is a contrivied example, but hopefully it shows the capabiliy of the `let` function.  The `let` function is probably more commonly used to execute a lambda on a non-null object.

```kotlin
@Test
fun `should execute code block for non-null values`() {
    // Given 
    val optionalVal: String? = "Hello"
    
    // When
    val message = optionalVal?.let { "$it World!" }
    
    // Then
    assertThat(message).isNotNull().isEqualTo("Hello World!")
}
```

In the previous example, the code uses the safe operator (i.e., `?.`) in Kotlin in combination with the `let` function to execute a lambda on a non-null object.

```kotlin
@Test
fun `should not execute code block for non-null values`() {
    // Given 
    val optionalVal: String? = null

    // When
    val message = optionalVal?.let { "This shouldn't be called." }

    // Then
    assertThat(message).isNull()
}
```

In the previous exampe, the lambda for the `let` function isn't executed so the `message` variable is null. 


### Run

The `run` function is Kotlin is commonly used for object creation and computing a result. A good example for this would be instantiating a service, configuring the properties (e.g., ports), executing the service, and return the results.

```kotlin
val service = HttpService()

val result = service.run {
    port = 8888
    context = "/acme"
    fetchResults()
}
```

The following is a simple unit test that can be used to experiment wth the `run` function.

```kotlin
@Test
fun `should use _this_ as object reference and return lambda result using _run_ function`() {
    // Given
    val numbers = mutableListOf<Int>()

    // When
    val count = numbers.run {
        add(1)
        add(2)
        count()
    }

    // Then
    assertThat(count).isEqualTo(2)
}
```


### With

```kotlin
@Test
fun `should use _this_ as object reference and return lambda result using _with_ function`() {
    // Given
    val numbers = mutableListOf<Int>(1,2 )

    // When
    val count = with(numbers) {
        count()
    }

    // Then
    assertThat(count).isEqualTo(2)
}
```

### Apply 

```kotlin
 @Test
fun `should use _this_ as object reference and return context object using _apply_ function`() {
    // When
    val numbers = mutableListOf<Int>().apply {
        add(1);
        add(2)
    }

    // Then
    assertThat(numbers).containsExactly(1, 2)
}
```

### Also

```kotlin
@Test
fun `should use _it_ as object reference and return context object using _also_ function`() {
    // When
    val numbers = mutableListOf<Int>().also {
        it.add(1);
        it.add(2)
    }

    // Then
    assertThat(numbers).containsExactly(1, 2)
}
```

## Summary

I hope this blog post has provided some insight into _scope functions_ within Kotlin. If you want to expirment with any of the examples, they can be found [here](https://github.com/seanking/scope-functions-kotlin) in GitHub.

