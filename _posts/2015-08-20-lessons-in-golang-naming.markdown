---
layout: post
title: "Lessons in Golang Naming"
date:   2015-08-20 12:14:44 -0600
---
> Dear fellow Gophers,
>
> I've been exploring [Go] recently, and I've been incredibly impressed by the community's ability to read my mind. I must be nuts, but it seems you all stumble across my thoughts long before I do. Acronyms and idioms polish the libraries that you distribute. 
>
> My life would be so boring without the hours that I spend trying to figure out what something like `dst` means (in standard library). Or what about, `reqPar`? It's obvious that this stands for RequestParameters. 
>
>Of course, it should be quite simple to just look at the Godoc, but this assumes that all developers document their code. Good one, guys! 
>
> This has been driving me nuts. Isn't it great? It's like playing a crossword and programming at the same time!
>
> Love, nuttyGopher13

Naming is brutal, but it's a key to build a maintainable code base. This post serves as a confession and a plea. During my internship in Bellingham, WA, I learned the importance of proper naming conventions. When I dove into some Go libraries, all of these lessons were suppressed.  I fell into the trap of using acronyms and idioms throughout my Go projects. While these shortcuts were learnt from other gophers, I still was responsible. I didn't even know why I was doing it. I wanted to be one of the cool gophers. You know, with a gold-plated tooth. 

Code quickly became incredibly disturbing. Sometimes I felt like I was communicating with the dead, not writing for the living. I sometimes wanted to dress up, take my Intel-powered Mac to a fancy restaurant, plug it in to one of those gold-plated sockets, and plea for forgiveness.

Ok, so, maybe, I can exaggerate. Either way, it looked bad. I decided to bring the conventions from my internship to all programs that I write. 

Before I start, let me say that **I'm not throwing Go under the bus.** The language and its community intrigues me. I'm still trying to wrap my mind around everything. Instead, it was fresh on my mind and some libraries were bothering me. I'm incredibly proud of Docker's source code. It is ultra-readable. I cannot recall struggling with what something was or what it did. 

Here's what I have adopted…

#### Don't use acronyms unless 7 billion people know them.
This one makes sense. If I create a Natural Language Processor Type, I should name it a `NaturalLanguageProcessor`, not `NLProc`. The first provides instant recognition to its context. As for the latter, most would need to refer to the docs. 

`HyperTextTransportProtocolRequest` is an exception. Everyone knows what *http* references. On a similar note, everyone is familiar with JSON, YAML, and XML. I do not plan to write these monstrous names out. I hope you do not, either.  Use your best judgment. 

#### Always start a function with a verb or iterator. 
Starting with either a verb or iterator makes sense, because functions have a precise definition. 

In mathematics, a function *f* assumes that for each *x*, there exists one and only one *y* such that *f(x)=y*.  In other words, if I have an *addOne* function and I pass it 4, I will always get 5. 

Functional programming is derived from this concept. Every function should compute a value. There is only computation. 

Imperative programming and functional hybrids introduce side effects. Side effects are behaviors outside of a mathematical computation. For instance, writing to a file or closing a socket would be side effects. These have become necessary. 

Since each function in Go, an imperative language, should represent a behavior, they should be described using verbs. No adult says, "*My hair green.*" It's missing it's verb. Similarly, no one says "*orange()*"; they say "*eatOrange()*".  In math, we name things *x*,*y*, and *z* for simplicity. What would we name two third degree polynomial with different coefficients? We only deal with a handful of functions at a time in math. However, we can deal with hundreds of thousands in software engineering. 

Use *is* or *has* whenever *true* or *false* is returned. Use words like *next* and *last* to indicate the progressive or successive return values. 

To make my case, here are some examples. Which sounds better?

```
Without -> With
------------------------
flagged -> isFlagged
fib -> nextFib
date -> getDate
left -> moveLeft
dbConnection -> openDbConnection
```

#### If possible, avoid suffixing a variable with its data type. 
Good variable names do not usually need additional words to describe the type of data they hold. In other words, don't include the data type in the name. If you find yourself doing so, reevaluate your naming options. Take these examples…

```
// redundant
todaysDate := getDate()

// concise
today := getDate()
```

When you hear a variable named `today`, what is the data type that pops in your mind?  It better be a date.  Similarly, take `infoString`. Ok, we get it is some sort of string, but what is info? This name is a cop out. Perhaps, this variable represents credentials to connect to a database. A better name would be `credentials`. 

Ask, "What does this contextually contain?"  You should rarely need to include the data type again. 

#### Always use qualifiers as a prefix and quantifiers as a suffix. 
To be fair, this section is more about consistency. Perhaps, your coding guidelines differ. Do what you believe is right. The force is with you. 

Whenever using a qualifier like *cached*, *new*, or *queued*, place it at the beginning of a variable's name. This follows our standard for most adjectives in English. The variable `fileCached` is deceitful. Is it a cached file or a flag indicating that the file is cached? Placing the qualifier at the front clears away any confusion: `cachedFile`. 

On the other hand, quantifiers, like *count*, should always be the suffix. For instance, `itemsCount` sounds fine, but `countItems` sounds like a behavior. You are forced to put an unnecessary "of" to remove the ambiguity: `countOfItems`. This also provides a disadvantage with autocomplete. If you have an array of `items`, you will problem type `items` out of a force of habit. If you are like me, you might miss `countOfItems` and make a redundant call to `len(items)`. 

In conclusion, if we are gophers, we are not nuts. If we are Java developers, we are not beans. If we are Groovy-developers, we are not stoned. We need to remain consistent. Naming is difficult for me, but I'm hoping this will be a great reminder. I hope it will do the same for you. 

Special thanks to my friends at Faithlife in Bellingham, WA who first introduced me to coding guidelines and helped me recognize the importance of naming conventions. 


[Go]: http://golang.org
