---
title: Exception Handling
sidebar:
  order: 14
slug: program-construction/exception-handling
prev: true
next: true
---

Exception handling in programming helps manage and respond to error conditions
gracefully. Instead of having programs crash when something goes wrong,
exceptions let us detect and handle errors in a controlled way.

Most languages support try-catch blocks for exception handling.

## Error-handling in java

In Java, runtime issues are denoted by objects of the `Throwable` class. The two
main categories of `Throwable` are `Error` and `Exception`.

### Error

Unrecoverable problems that are not meant to be caught.

Examples: `OutOfMemoryError` or `StackOverflowError`.

### Exception

#### Checked Exceptions

Checked at compile time.

Examples: `IOException`, `SQLException`.

#### Unchecked Exceptions

Not checked while compiling.

Examples: `NullPointerException`, `ArrayIndexOutOfBoundsException`.

### Try-catch block

```java
try {
    // Code that might throw an exception
    int result = 10 / 0; // Will throw ArithmeticException
} catch (ArithmeticException e) {
    // runs if there is an arithmetic exception
    System.out.println("Cannot divide by zero!");
} catch (Exception e) {
    // runs if there is any other type of exception
} finally {
    // runs whether an exception occurs or not
    // runs whether the exception is caught or not
}
```

The code that might cause errors is placed inside a `try` block. If an exception
occurs, the code inside the `catch` block is executed. The `catch` block defines
what type of exception to catch and what to do when it occurs. Multiple `catch`
blocks can be defined to handle different type of exceptions. `catch` block has
access to an object of `Exception` class, which defines the error that occurred.

The `finally` block is used for code that should run whether an exception occurs
or not. Finally block has some special behaviours as mentioned in
[this answer on StackOverflow](https://stackoverflow.com/questions/3354823/why-use-finally).

Exceptions can be thrown using `throw` keyword.

```java
public void validateAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Age cannot be negative");
    }
}
```

Custom exceptions are defined by extending the `Exception` class.

```java
public class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}
```

Methods that might throw exceptions should declare this using the `throws`
keyword:

```java
public void riskyMethod() throws CustomException {
    // Method implementation
    if (someErrorCondition) {
        throw new CustomException("An error occurred");
    }
}
```

### Exception Hierarchy

```txt
Throwable
├── Error
│   ├── VirtualMachineError
│   │   ├── OutOfMemoryError
│   │   └── StackOverflowError
│   └── AWTError
├── Exception
│   ├── RuntimeException (Unchecked)
│   │   ├── NullPointerException
│   │   ├── ArithmeticException
│   │   └── IndexOutOfBoundsException
│   └── IOException (Checked)
│       ├── FileNotFoundException
│       └── EOFException
```
