---
layout: post
title:  "Why ECMAScript 6 is like Nirvana"
date:   2015-05-15 12:14:44 -0600
---
I started a new position as a software development intern at Faithlife in Bellingham, WA.  When it comes to languages, the overwhelming consensus is to use C# and JavaScript at the company.  Believe me, I know.  As a Ruby-aficionado, I keep wanting to bask in its syntactic paradise.  From optional parenthesis to inline conditionals to blocks and procs to a variable type inference without the `var` keyword to beautiful loops to elegant operating overloading to substring to named parameters in 2.0, Ruby is one sexy jewel. I think I just invented code-lust.  Don't worry, I will totally remember to edit this out before I publish.

Anyway, I have been assigned projects in the Node realm.  I'm actually happy, because I like JavaScript (somewhat).  However, let's be real. There are so many things that just make it burdensome.  Most of these are style preferences.  But seriously, I have to look at it all day, though.  I feel this personal connection with ECmAScript 6.  Let's look at what I love...

## Arrows
Basically, the standards body came to the realization that typing out `function` every two seconds can become burdensome.  This is especially the case with callbacks.  

Here's an example...
{% highlight javascript %}
// Before EMCAScript 6
getUserData(sanitizedArguments, function (data) {
  updateDisplay(data);
});

// After EMCAScript 6
getUserData(sanitizedArguments, (data => updateDisplay(data));
{% endhighlight %}

Isn't that jaw-dropping-ly gorgeous?  It reminds me of the lambda syntax in C# (which I adore).  I truly believe that this is a great step in the right direction.  Note that Arrows do infer **`this`** as being the scope of their surroundings.

## Classes
Let's be real... there's nothing remarkably new behind the scenes.  That being said, it's about time that JavaScript recognize a class and its structure in the standard way all other Object Oriented Languages do.  We have constructors, properties, classes with inheritance,  and static declarations.  This is amazing for so many reasons, but think about project structure...

One of the uggliest things about different JavaScript projects is that they take on so many different structures.  Think about all of the ways to define an object.  Who thought that wrapping a class in a function was a good idea?

## String Interpolation
This should have come years ago.  It was one of the reasons that I tried using CoffeeScript for a while.  Now, it's so beautiful...

{% highlight javascript %}
// Before
'Hi ' + name + ', how are you this ' + dayOfWeek + '?'

// After
`Hi ${name}, how are you this ${dayOfWeek}?`
{% endhighlight %}

It's something small, but it has become a necessary convenience in 21st century programming.

## Modules and Imports
No more crazy workarounds.  ECMAScript 6 supports modules and imports.  JavaScript is finally becomming natively modular.  No, I do not believe it was before. Grouping things under object literals which would be wrapped in other objects and functions is more of a workaround.  It's not as clear-cut as explicit namespaces and modules in other languages.  Anyway, here's an example of the new import syntax...

{% highlight javascript %}
// RequireJS
define([
  'util/math',
  'util/logger'
], function (math, logger) {
  // do something with math and logger
});

// Standard NodeJS Imports
var math = require('./util/math'),
    logger = require('./util/logger');
// do something with math and logger

// EMCAScript 6
import {math, logger} from 'util';
// do something with math and logger
{% endhighlight %}

And exports are just as elegant.

## Promises
There is really nothing new here.  We've all taken advantage of Promises or a module that depends on some sort of Promise.  Now, we are simply incorporating it into the language.  Hopefully, this will encourage the use of promises over the anonymous, asynchronous callback functions that tend to morph into spaghetti code over time.

As a developer who has dabbed in practically every language in which I have encountered a compiler or interpreter able to run on my machine, I have a lot of expectations.  I'm proud to say that EMCAScript 6 has exceeded these expectations on paper.  In the past, I dreaded working extensively in JavaScript for large projects.  Now, I'm raising an eyebrow.  It all comes down to how the standard is implemented in the innumerable interpreters.  If it is done right, JavaScript may soon make the top of my list.

If you are interested in learning more without reading the spec, [this repo][0] gives an overview in a nice markdown file.  I love it, because it makes a great reference.  It won't consume too much of your time. If you want to check out the spec, [the latest is here][1].  

Come on members.  Vote on the final draft!  We may have found Nirvana, and I'm not talking about any teen spirit.

[0]: https://github.com/lukehoban/es6features
[1]: https://people.mozilla.org/~jorendorff/es6-draft.html
