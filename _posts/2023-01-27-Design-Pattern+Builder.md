---
layout: post
title: "Design pattern: Builder"
description: "Builder is a creational design pattern that lets you construct complex objects step by step."
tag:
  - swift
  - design patterns
---

# 👷 Builder
Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

Let's say that we have below class which holdes a few fields which can be set under `init` method:

``` swift
import Foundation

public class HTTPRequest {
    public var url: URL?
    public var headers: [String: String]?
    public var method: HTTPMethod?
    public var path: String?
    
    public init() {}
}
```

## Solution 1: HTTPResponseBuilder
'Old' fashioned way :)

``` swift
import Foundation

public final class HTTPMethodBuilder {
    var method: HTTPMethod
    
    var url: URL? = nil
    var headers: [String: String]? = nil
    var path: String? = nil
    
    public init(method: HTTPMethod) {
        self.method = method
    }
    
    @discardableResult
    public func with(url: URL) -> Self {
        self.url = url
        return self
    }
    
    @discardableResult
    public func with(headers: [String: String]) -> Self {
        self.headers = headers
        return self
    }
    
    @discardableResult
    public func with(path: String) -> Self {
        self.path = path
        return self
    }
    
    public func build() -> HTTPRequest {
        return HTTPRequest(builder: self)
    }
}
```

If we don't want to allow anyone else to create our object in a different way we can mark our init method as private and leave only the following one:

``` swift
public extension HTTPRequest {
    
    convenience init(builder: HTTPMethodBuilder) {
        self.init()
        url = builder.url
        headers = builder.headers
        method = builder.method
        path = builder.path
    }
}
```

Usage may look as follow:

``` swift
let httpRequestSolution1: HTTPRequest = HTTPMethodBuilder(method: .head)
    .with(url: URL(string: "https://aweseome.builder.example.io")!)
    .with(headers: [
        "Content-Type": "application/json"
    ])
    .with(path: "/all")
    .build()
```

## Solution 2: Injectable closure
Less code but it doesn't mean that it's better. Here, this approach has a couple of issues. First, we need to update our model as one of our parameters is marked with `let`. Another, which is not a case for this example but to declare the injectable closure that will be capable of initializers all the properties outside of the target's object, we actually break the encapsulation. Also, in order to be able to set new values, all properties may be required to be public.

``` swift
public extension HTTPRequest {
    
    typealias HTTPRequestBuilderClosure = (HTTPRequest) -> Void
    
    convenience init(builderClousre: HTTPRequestBuilderClosure) {
        self.init()
        builderClousre(self)
    }
}
```

And the usage may look as follow:

``` swift
var httpRequestSolution2: HTTPRequest = HTTPRequest() { (httpRequest: HTTPRequest) in
    httpRequest.method = .head
    httpRequest.url = URL(string: "https://aweseome.builder.example.io")
    httpRequest.path = "/all"
    httpRequest.headers = [
        "Content-Type": "application/json"
    ]
}
```

## Solution 3: Keypath Builder
Available since the introduction of Swift's Key Path addition with Swift 4.0 release. May be quite generic one, but also may be quite dangerous since we can easily misspell a keypath name and get hated run-time error! Be careful, as usual ;)

``` swift
protocol Builder {
    /* empty by purpose as implementation is added under extension */
}

extension Builder where Self: AnyObject {

    @discardableResult
    func set<T>(_ property: ReferenceWritableKeyPath<Self, T>, with value: T) -> Self {
        self[keyPath: property] = value
        return self
    }
}
```

Next, to use such as builder we need to add a conformance to `Builder` protocol to the type that needs to get this functionality, in our example for `HTTPRequest`.

``` swift
extension HTTPRequest: Builder {}
```

 Then, we could use it as follow:

``` swift
let httpRequestSolution3 = HTTPRequest()
    .set(\.url, with: URL(string: "https://aweseome.builder.example.io"))
    .set(\.path, with: "/all")
    .set(\.headers, with: ["Content-Type": "application/json"])
```

## Solution 4: More advance usage using some Builder protocol + Director
A way to make our builder implementation more advanced, is to create a builder as a protocol, and having different concrete builder classes to implement necessary builder protocol functions, such as `build()`. Using additional component like Director is useful, when producing objects according to a specific order or configuration. A Director takes a builder as a parameter and it is only responsible for executing the building steps in a particular sequence.

## Solution 5...:
No surprise, that there is no one proper solution when it comes to writing code. Also here, there are more variations of implementing of builder pattern.

## 📓 Conclusion
Remember, it's always up to you (well, sometimes maybe from architect or some senior guru in your project ;)) to decide which approach suits best for your particular case and context.

[Here](https://github.com/polok/design-patterns-with-swift/tree/main/Gang-of-Four/Creational/Builder) is a playground with the above source code.

## Related design patterns:
* Abstract factory

As our builder, it can construct complex objects. The main difference is that builder pattern emphasises step-by-step creation of objects. Abstract factory emphasises on product families creation of objects. Builder returns the object in last step (usually under our build function) where abstract factory returns it immediately.