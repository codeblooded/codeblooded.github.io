---
layout: post
title:  "Migration to a Ghost Blog"
date:   2014-05-15 12:14:44 -0600
---
Recently, I decided several things.  First, I was tired of the limitations that I came across with hosting my blog as a page on GitHub.  I'd like to install some plugins, bring in some more themes, and, eventually, support searching through my posts.  All of these are *very* limited on GitHub pages, because I cannot access the server (and everything is static files).

I did attempt to give the famous Wordpress a try; however, it felt cluttered and there were so many distracting elements that took my attention away from my content.  Also, I was bothered by the lack of support to preview Markdown.

As I was planning on setting up my site, I decided to spin up a server, or *droplet*, on [DigitalOcean](http://digitalocean.com). In the past, I have used them several times.  They continue to impress me with their remarkable service for the cost. I like that I can  get a 512MB server for only $5/month. 

Now, let me clarify on GitHub pages.  I absolutely adore GitHub, and I'm a loyal Octocat. Meanwhile, I have found that pages seem to be the most effective as a single page site explaining a project or bio of the developer.  Because there is no backend support with a database, it complicates the amount of customization available when it comes to search optimization. 

While Ghost, unlike Wordpress, does not have builtin support for search, there are several projects out there which have used various techniques to incorporate search.  Some index the RSS feed.  Either way, I'm happy.

Finally! I found a solution that let's me use Markdown.  I get a full preview to the right as I type, and it is not cluttered.  Best of all, I can put some other content on my server (which I can fully access). It's a much better setup.
