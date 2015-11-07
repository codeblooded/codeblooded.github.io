---
layout: post
title:  "Dropping the TabBar, Thanks Human Interface Assumptions"
date:   2014-05-24 12:14:44 -0600
---
Today, I was working on a project, and I came accross an unfortunate assumption.  `UITabBarController`'s consume and control all other view controllers which 'belong' to it.  Apparently, it has some nasty bugs when it comes to the new iOS 7 animation APIs and trying to present an additional view controller modally.  

For instance, I wanted a simple, pop-over transition for creating a new element in a `UITableView` inside of a `UITableViewController` which was part of the `UITabBarController`.  Anyway, I conformed to the new animation protocol: `<UIViewControllerTransitioningDelegate>`, and my animation worked perfectly.

Unfortunately, after my animation completed, an Exception was thrown which made me scratch my head (and tear up a little): 

> *** Terminating app due to uncaught exception `'NSInvalidArgumentException'`, reason: 'Application tried to present modally an active controller [...].'

After a quick Google search, I realized that other developers had the same issue. An assumption was made as to how animations should be handled when it comes to `UIViewControllers` which are part of a tab bar controller.

Now, I will be taking the **painful**, yet better approach.  I'm dropping the tab bar.  It's too cliché anyway, so I might as well try to come up with a more simplistic, modern user experience which let's my app stand out on the store.