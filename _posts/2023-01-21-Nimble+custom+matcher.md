---
layout: post
title: "Nimble: a custom matcher for a rescue"
description: "Create your very own matchers to encapsulate some complex form of matching, or to increase the readability of some of your existing tests"
tag:
  - swift
  - testing
---
This post is not going to explain what is [Nimble](https://github.com/Quick/Nimble). I can only say that I and many other iOS devs are using it to express the expected outcomes of Swift expressions in their tests. If you want to learn more, go [here](https://github.com/Quick/Nimble).


## Nimble Matchers
A Nimble matcher is a function that allows you to describe
* the positive case (the values match), 
* the negative case (they don’t match),
* any error cases all in one function.

This means that not only can you write `expect(value).to(equal(expectedValue))`, but you can also reuse the same matcher for the negative case `expect(value).toNot(equal(exptectedValue))`. Nice, right?

Examples:

``` swift
import Nimble

expect(1 + 1).to(equal(2))
// success - expected to equal <2>, and got <2>

expect(1+1).toNot(equal(3))
// success - expected <2>, to not be equal <2>
```

Let's, have a look at how such a matcher looks in code. Btw, [here](https://github.com/Quick/Nimble/tree/main/Sources/Nimble/Matchers) is the list of already defined matchers.

![](/images/2023-01-21-Nimble+custom+matcher/nimble-beEmpty-defined-matcher.png)

In Nimble, the matchers are Swift functions that take an expected value and return a `Predicate` closure. The return value of a `Predicate` closure is a `PredicateResult` that indicates whether the actual value matches the expectation and what error message to display on failure. The expression represents the closure of the value inside `expect(...)`.

### Predicate

``` swift
public struct Predicate<T> {
    fileprivate var matcher: (Expression<T>) throws -> PredicateResult

    /// Constructs a predicate that knows how take a given value
    public init(_ matcher: @escaping (Expression<T>) throws -> PredicateResult) {
        self.matcher = matcher
    }

    /// Uses a predicate on a given value to see if it passes the predicate.
    ///
    /// @param expression The value to run the predicate's logic against
    /// @returns A predicate result indicate passing or failing and an associated error message.
    public func satisfies(_ expression: Expression<T>) throws -> PredicateResult {
        return try matcher(expression)
    }
}
```

### PredicateResult

It is the return struct that `Predicate` return to indicate success or failure. A `PredicateResult` is made up of two values: 
* PredicateStatus
* ExpectationMessage

#### PredicateStatus

``` swift
public enum PredicateStatus {
// The predicate "passes" with the given expression
// eg - expect(1).to(equal(1))
case matches

// The predicate "fails" with the given expression
// eg - expect(1).toNot(equal(1))
case doesNotMatch

// The predicate never "passes" with the given expression, even if negated
// eg - expect(nil as Int?).toNot(equal(1))
case fail

// ...
}
```

#### ExpectationMessage
It provides messaging semantics for error reporting.

``` swift
public indirect enum ExpectationMessage {
// Emits standard error message:
// eg - "expected to <string>, got <actual>"
case expectedActualValueTo(/* message: */ String)

// Allows any free-form message
// eg - "<string>"
case fail(/* message: */ String)

// ...
}
```

Predicates should usually depend on either `.expectedActualValueTo(..)` or `.fail(..)` when reporting errors.

## Custom matchers in practice

There are [a few ways](https://github.com/Quick/Nimble/blob/main/Sources/Nimble/Matchers/Predicate.swift) how we can init our own matchers but I will focus on two which I use the most:

``` swift
/// Defines a predicate with a default message that can be returned in the closure    
/// Also ensures the predicate's actual value cannot pass with `nil` given.

public static func define(_ message: String = "match", matcher: @escaping (Expression<T>, ExpectationMessage) throws -> PredicateResult) -> Predicate<T>
```

``` swift
/// Provides a simple predicate definition that provides no control over the predefined    /// error message. Also ensures the predicate's actual value cannot pass with `nil` given.

public static func simple(_ message: String = "match", matcher: @escaping (Expression<T>) throws -> PredicateStatus) -> Predicate<T>
```


Let's say that we have an enum that describes http method with some associated values:

![](/images/2023-01-21-Nimble+custom+matcher/httpmethod_enum_case.png)

We want to have the matcher/s which allows us to write a neat test. First, a get matcher for which we will use a `define` method.

![](/images/2023-01-21-Nimble+custom+matcher/matcher_be_get.png)

Example of usage:

![](/images/2023-01-21-Nimble+custom+matcher/matcher_be_get_example.png)

For the second example, we will use `simple` method:

![](/images/2023-01-21-Nimble+custom+matcher/matcher_be_put.png)

Example of usage:
![](/images/2023-01-21-Nimble+custom+matcher/matcher_be_put_example.png)

## Summary

As we can see in the above examples. Writing our own Nimble matcher requires us to write more code than the pattern-matching solution. However, in the end, the test is much more flexible when it comes to checks which we want to make and is easier to read.