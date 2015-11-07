---
layout: post
title:  "When a Single Response Doesn't Work"
date:   2014-05-16 12:14:44 -0600
---
I recently began work on a new project, and I'd like to tell you more. Unfortunately, I'm not at liberty to do that at this time. However, I will describe one of the significant problems that I have to face. As soon as I deploy the project and patents are issued, I will blog all about it. Anyway, let's get back to the problem at hand. 

### Request and Response
Alright, I've gotta be honest. I've always seen and worked with a request being sent, some processing on the server, and then a response is sent as the communication is cut off. 

This is not a problem, and it has worked for years. As a matter of fact, this is standard protocol. 

If you look at any social network, this is how events are processed: one at a time. And thousands, potentially millions, are processed every second. However, the same devices are re-establishing the connection and dropping it over and over again. How redundant!

### The 21st Century
Now, people want things to happen instantly. Everything needs to occur constantly, and the requests no longer need to just be known between the client and server, but also the other clients. They need to be continually updated to maintain interactivity. That is *precisely* why sockets are the future. 

The other alternative that others seem to find is polling for changes. This is also rediculous. We are still breaking off the connection. In an effort to continue the old methodology, we've created a terrible practice. 

So, when it came time for this project, we abandoned what we knew and tried something new. 

### Persistant Sockets, Temporary Data
So, as you've probably assumed, we have developed a new system of communication with persistant sockets. Now, let's be honest. The challenge is keeping track of the varying status changes with persistent connections on a mobile device. We need a continually updating system which checks: 

- Are you still there? 
- Can you receive a transmission of data without significant packet loss? 
- Are you still authenticated with a valid key to access third party APIs? 
- If the connection was lost, can we resume the session without problems?

There are so many things that can interfere with a persistent socket communication between a server and mobile phone. The phone can switch from WiFi to cellular, it could enter a dead zone before, during, or after a transmission. 

How do we process the data so quickly? [Redis](http://redis.io) is the key!  By being an in memory database, it is remarkably fast. The data that we collect needs to only be active for the current user session in order for the application to function properly. Also, the redis pub and sub feature has simplified our overhead. We rely on this to provide the clients with information from other clients. 

Now, you might be thinking an in memory database sounds like a terrible idea, because there is data that you would like to persist. Perhaps, the data could be useful in the future. We know that!

Our solution is background jobs. When the servers are idle, or processing subsides, we can run jobs in the background to transfer the Redis data to a more persistant store. 

To transition to data size, we continue to use MessagePack—an efficient modification to JSON—when using sockets. This allows us to easily parse the data. We follow a *very* conservative approach when it comes to the size of data transmitted over the socket. By using the least possible, it is less likely that more data will be lost. 

That's it. We'll see how it runs at launch. 
