---
layout: post
title: Futures in Java
redirect_from: news/2016/4/7/futures-in-java/
---
# Futures in Java

Recently, I have been reading about the [Lagom Framework](https://www.lightbend.com/lagom) for micro-services. In the Introduction to Lagom video by James Roper, he mentions that the [Future API in Java](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html) only allows for a Future to be interacted with synchronously (i.e., thread blocking). Since I previously have only used [Futures in Scala](http://www.scala-lang.org/api/current/#scala.concurrent.Future), this came as a surprise because the Future API in Scala is by default non-thread blocking. The following sections cover the basic difference between the Future and CompletableFuture APIs.

## Set Up

The following _getGreeting()_ method will be used asynchronously throughout this post. The method will simply return "Hello World!" after sleeping for a second.

```java
private String getGreeting() {
  System.out.println("Starting async method...");
  try {
    Thread.sleep(1000L);
  } catch (InterruptedException e) {
    e.printStackTrace(); // Forgive me
  }
  return "Hello World!";
}
```

## Future

A Future as defined by the Java Documentation: "_A Future represents the result of an asynchronous computation. Methods are provided to check if the computation is complete, to wait for its completion, and to retrieve the result of the computation. The result can only be retrieved using method get when the computation has completed,_ __blocking if necessary until it is ready.__"


The following is an example of using the Future API. First, the [ExecutorService](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html) is used to create a [FutureTask](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/FutureTask.html) (an implementation of Future) for the result of the _getGreeting()_ task. The result of the task is obtained by using [Future.get()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html#get--). Finally, the greeting is printed to the console.

```java
public void sayHelloWithFuture() throws Exception{
  final ExecutorService executor = 
        Executors.newFixedThreadPool(1);

  Future<String> futureMessage = executor.submit(() -> {
    return getGreeting();
  });

  System.out.println(futureMessage.get()); // blocks thread
  System.out.println("Goodbye!");
}

// Order of the output
// 1. Starting async method...
// 2. Hello World!
// 3. Goodbye!
```

It is important to note that Future.get() will block the thread until the asynchronous task has completed.

### Completeable Future

Java 1.8 introduced a [CompletableFuture](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html) class that improves asynchronous support in Java. CompletableFuture is an implementation of the Future interface that utilizes callbacks to handle the results of an asynchronous operation. The CompletableFuture also implements the [Com]pletionStage](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletionStage.html) interface. The CompletionStage interface provides a functional and fluent API for working with asynchronous events.

The following example refactors the previous example to use a CompletebleFuture. The [CompletableFuture.supplyAsync(...)](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html#supplyAsync-java.util.function.Supplier-) creates a task that calls _getGreeting()_ and returns a CompletableFuture. The completion of the asynchronous task will result in the [CompletableFuture.thenAccept(...)](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html#thenAccept-java.util.function.Consumer-) method being called with the results of _getGreeting()_.

```java
public void sayHelloWithCompleteableFuture() throws Exception {
  CompletableFuture
    .supplyAsync(this::getGreeting)   // Create async task
    .thenAccept(System.out::println); // Print results to console

  System.out.println("Goodbye!");
}

// Order of the output
// 1. Starting async method...
// 2. Goodbye!
// 3. Hello World!
```

An interesting side effect of the refactoring is that "Hello World!" is being printed last. This occurs because the thread does not have to wait for the results of the asynchronous _getGreeting()_ task to print "Goodbye!".

The example can be refactored again to provide similar results as the first example by adding "Goodbye!" to the execution chain. The CompletionStage provides an interface to chain multiple asynchronous and synchronous tasks together. However, the chaining of tasks is a subject for another post.

```java
public void sayHelloWithCompleteableFuture() throws Exception {
  CompletableFuture
    .supplyAsync(this::getGreeting)
    .thenAccept(System.out::println)
    .thenRun(() -> {
      System.out.println("Goodbye!");
    });
}


// Order of the output
// 1. Starting async method...
// 2. Hello World!
// 3. Goodbye!
```

It is important to note that since the CompletableFuture does implement the Future API, therefore using CompletableFuture.get() will block the thread until the task has completed.


### Conclusion

The CompletableFuture has brought Java's asynchronous capabilities more inline with other languages that are being used to build reactive systems. Other than testing, the Future API appears to have been made obsolete by the CompletableFuture class and CompletionStage interface.

The Java examples and a Scala example can be found in [futures-jvm](https://github.com/seanking/futures-jvm) on GitHub.