---
layout: post
title:  "Peculiar Behavior with Interface Types and Variadic Arguments"
date:   2016-01-16 02:29:44 -0600
---

Variadic arguments are nothing new.  They power all of our `Printf` statements.  These are just syntactic sugar for arrays.  For example, here is a variadic function that prints all of the strings passed to it:

{% highlight go %}
func printStrings(strs ...string) {
  for _, v := range strs {
    fmt.Println(v)
  }
}
{% endhighlight %}

So, if I call `printStrings("one", "two", "ten")` or `printStrings("one", "twelve")`, it will print everything out!

In Golang, `interface{}` is a valid type.  Everything conforms to the empty interface, so it can represent any object.  So, this is true:

{% highlight go %}
  var here interface{} = []string{"hi"} 
{% endhighlight %}

And this is, too...

{% highlight go %}
  i := 5
  var ii interface{} = i
  var iii interface{} = &ii
{% endhighlight %}

So, what happens if I attempt to wrap a variadic function with another variadic function?

If they are both of type `interface{}`, it works (with a caveat):

{% highlight go %}
package main

import (
  "fmt"
  "reflect"
)

func main() {
  fmt.Println("A direct call to a function with variadic arguments:")
  direct("one", "two")
  
  fmt.Println("\n\nAn indirect call to a function with variadic arguments:")
  indirect("one", "two")
}

func indirect(ok ...interface{}) {
  fmt.Printf("%20s %v\n", "Middleman Data=", ok)
  direct(ok)
  fmt.Printf("%20s %v\n", "Indirect Type=", reflect.TypeOf(ok))
}

func direct(ok ...interface{}) {
  fmt.Printf("%20s %v\n", "Data=", ok)
  fmt.Printf("%20s %v\n", "Direct Type=", reflect.TypeOf(ok))
}
{% endhighlight %}

Output:

<pre>
A direct call to a function with variadic arguments:
               Data= [one two]
        Direct Type= []interface {}


An indirect call to a function with variadic arguments:
     Middleman Data= [one two]
               Data= [[one two]]
        Direct Type= []interface {}
      Indirect Type= []interface {}
</pre>

Specifically, the second call is nesting an array of `interface{}`s:

{% highlight go %}
Middleman Data= [one two]
          Data= [[one two]]
{% endhighlight %}

However, both show the type `[]interface{}`, because a slice of `[]interface{}` is still an `interface{}`. Weird, right?

---------

In contrast, if I change it to specifically use strings, I get a nice error:

{% highlight go %}
func indirect(ok ...string) {
  fmt.Printf("%20s %v\n", "Middleman Data=", ok)
  
  direct(ok) // ERROR!
  
  fmt.Printf("%20s %v\n", "Indirect Type=", reflect.TypeOf(ok))
}

func direct(ok ...string) {
  fmt.Printf("%20s %v\n", "Data=", ok)
  fmt.Printf("%20s %v\n", "Direct Type=", reflect.TypeOf(ok))
}
{% endhighlight %}

Specifically, a slice of strings is not the same as passing arguments to the variadic function:

<pre>
prog.go:18: cannot use ok (type []string) as type string in argument to direct
</pre>

Of course, I could just use slices. This is not very different, it is just not very appealing:

<pre>
indirect([]string{"one", "two"})
</pre>

However, this  approach is much more extensible.  I can wrap it as much as I want without added complexity.

## Questioning Design

However, this really made me question Go.  It makes sense that the `interface{}` one works, but it can suppress important errors that may indicate a problem down the road.  Even using reflection, I cannot tell the immediate difference between `[]interface{}` and one nested within another. They are both of the `interface{}` type.

This has led me to be incredibly cautious when it comes to the `interface{}` type. This concept is far from strictly typed, and can introduce an array of problems into a program.  It seems to be used for one thing: *a compromise*.  It removes the need for generics, but, if it is used in excess, it undermines the beauty of Go's type system. 

What are your thoughts?  Let me know.
