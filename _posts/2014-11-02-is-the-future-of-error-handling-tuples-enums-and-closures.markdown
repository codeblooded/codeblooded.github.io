---
layout: post
title:  "Is the Future of Error Handling Tuples, Enums, and Closures?"
date:   2014-11-02 12:14:44 -0600
---
Apple's new language, *Swift*, lacks the ability to handle a thrown/raised exception.  Many people actually argue that this is a benefit.  I'll discuss two new error handling approaches and weigh their pros and cons.

#### Enums and Tuples
Let me start by saying that this seems to be the most popular form that I've seen in distributed swift code.  Basically, an enumeration is defined with cases representing the potential "side effects" of a certain operation or groups of operations.  For instance, an Arithmetic framework may define an enum as such:

```language-swift
enum Result<T> {
    case Success(Value<T>)
    case Error(NSError)
}
```
Then, when a certain function is called, for instance `divide(numerator: 5, denominator: 0)`, a `Result<T>` is returned.  In this case, it would return `.Error` with an error explaining that the system cannot divide by 0. 

An example of calling the divide function would be:
```language-swift
let result = divide(numerator: 5, denominator: 0)
switch result {
  case .Success(let value):
      // unwrap value (explained below)
    case .Error(let error):
      // handle the error
}
```
The unfortunate characteristic of this pattern is unwrapping.  Notice that the enum `Result` is generic.  We don't know if the arithmetic expression is going to return an *integer*, *double*, *float*, etc.  It all depends on the operands.  Therefore, this method of error handling requires us to unwrap the value.  In our prior example, an unwrap would might look like:
```language-swift
case .Success(let value):
  // assuming the actual type T is accessible through an
    // innerValue property.
  let unwrappedValue = value.innerValue
    println(unwrappedValue)
```
In our scenario, it just added a simple step: access the property on the Wrapped-Item.  However, this can become more complicated if you have different types or groups of types.   

Some express that one of the important behaviors this pattern invokes is that the developer is forced to account for some sort of error.  In order to even unwrap the final return value, you need to check the status (the enum) of the operation.  This reinforces the idea of handling errors in a responsible manner. Similary, this pattern is commonly used for values needed immediately after the invoke of the method.  Developers are more likely to implement an error response for a synchronous operation.

- **PROS**
  - Practically forces error handling
  - Obviously, doesn't kill the application
  - Provides a structure for organizing success and failure messages
- **CONS**
  - Complicates code readability due to defined enums
  - Requires unwrapping for generic types
  
#### Closures
When it comes to networking, the concept of a *callback* or *response to a certain event* is critical.  Over and over again, computer scientists have seen that the model of calling "if this occurs" is generally a better practice than simple declarative programming. 

One of the best examples of this power-house concept is [NodeJS](http://nodejs.org).  Everything that you do is driven by a callback.  Sockets can be kept open and messages can be pushed to the client.  The client's application only responds if and when a message is pushed.

Another example is the delegation pattern used by Apple.  Whenever a UITableView needs to populate itself, it repeatedly asks the data source, "what should go in here?"  Instead of the developer telling the system everything it must do, they provide methods that are called in the event that it ever happens.

Now, closures can be used for error handling.  This is especially prominent in networking and UI-driven actions.  Instead of defining a separate type, the function itself takes optional closures if events may arise.  For instance, `

```language-swift
func divide<T>(_ numerator : T, denominator : T, 
           success : ((T)->())?, error : ((NSError)->())?)
```

**Notice**: there is no return value.  The program does not rely on the immediate response of the operation.  It passes back the answer to the success closure.

> Obviously, this is not the only way.  There are situations   where a closure is used for error events, but a value is returned.  For now, we will focus on the pattern described.

- **PROS**
  - Obviously, doesn't kill the application
  - Provides a structure for organizing success and failure messages
  - Great for asynchronous events
- **CONS**
  - Does not generally enforce error handling (with optional types)[^1]
  - Causes problems with synchronous operations where values are immediately needed.

#### Hybrids

Welcome to Wonderland! Some projects are actually combining the enums and closures for error reporting systems.  This extends the scope of this blog post. However, I just thought I might throw this bit of knowledge out there.


[^1]: You could change this and create a strict system which forces the developer to handle errors through the a **non-optional!** However, the developer can also just implement the closure and not actually do anything with the error.  Similarly, this can be said for the tuple and enum method. However, it is more likely that the will handle it if they cannot unwrap an object.  Generally, in those situations, it is a synchronous problem which must be handled.