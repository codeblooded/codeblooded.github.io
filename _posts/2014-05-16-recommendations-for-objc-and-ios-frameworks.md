---
layout: post
title:  "Recommendations for Objective-C and iOS Frameworks"
date:   2014-05-16 12:14:44 -0600
---
Hey there folks! I'm ready to find out the changes that have been made to the Objective-C language and frameworks in the last year or so at WWDC.  However, I have a few recommendations up my sleeve which I *wish* would pan out.

> **Disclaimer.** 
> Some of these recommendations do not necessarily pertain to the language itself.  Some things have more to do with the compiler and Xcode, but I will try to focus on language aspects.

<hr />

#### Migration away from C in the Libraries 
This should be a given, but, if I'm writing in Objective-C, I'd like to have to stop converting objects between standard C and the new objects.  Seriously, using a `__bridge` with ARC is just annoying and clutters up my code.  There are some places where this isn't a problem, for instance, UIKit is *pretty* up-to-date.

However, look at **CoreGraphics** and **QuartzCore**.  I'm so tired of going from `UIColor` to `CGColorRef` and vice versa. It's a change that has tremendous backwork (when you think about all of the functions included in these libraries), but it makes the code more maintainable.

<hr />

#### Keep the Shortcuts Coming, but Don't Deprecate
I love the new shortcuts for things like instantiating an `NSArray` and filling it with objects or an `NSDictionary` with keys and values.  These shortcuts are *elegant*, and they do save a lot of time (as well as make it prettier).  The only thing that can cause problems is deprecating the old way of doing it.

<hr />

#### Extract Core Data from the App Delegate

While this is more of a template and best practice request, it is still prevalent to me.  I agree with Ben on [NSScreencast](http://nsscreencast.com)[^1], setting up Core Data does not belong in the AppDelegate.  

We all agree that reaching back to the AppDelegate for the context is a bad idea, but why go to the hassle of passing the context down the hierarchy? Instead, let's extract everything involving CoreData setup into a separate class.

<hr />

#### Trust the Developers, Don't Throw Exceptions based on Assumptions

As I wrote about in  [Dropping the TabBar, Thanks Human Interface Assumptions](http://www.codeblooded.me/dropping-the-tabbar-thanks-human-interface-assumptions/), I don't particullary like the idea of exceptions being called due to the fear that it might not, for lack of a better expression, look *appropriate*. Part of our job as developers is to determine what provides the best experience for our users on the App Store.  While I see where Apple is coming from, I think that they should trust us to make some of these interface decisions and face the consequences.

<hr />

#### Bring back Mass-Support for `-(NSObject) new`
We have all been used to the verbose syntax with `alloc` and `init`.  It seems that there is a trend, especially with open source projects to revert to this shortcut.  There are times when we need to allocate an object, but not initialize.  Therefore, this syntax is very effective.  Meanwhile, I think that messaging  `new` should be encouraged for its simplicity.

<hr />

Right now, that's all that I can really wrap my mind around.  I'll probably publish this, wait a couple hours, and then remember something.  *How could I forget that?!* Anyway, let me know what you would like to see.

<hr />

[^1]: [NSScreencast](http://nsscreencast.com) is a weekly, subscription podcast which covers aspects relevant to iOS Development, similar to Railscasts and Ruby on Rails.  I highly recommend it.