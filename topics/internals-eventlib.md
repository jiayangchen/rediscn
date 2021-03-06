---
layout: topics
title: REDIS internals-eventlib
permalink: topics/internals-eventlib.html
disqusIdentifier: topics_internals-eventlib
disqusUrl: http://redis.cn/topics/internals-eventlib.html
discuzTid: 876
---

Event Library
===

为什么需要Event Library?
---

让我们通过一系列Q&A来寻找答案

Q: 你期望一个网络服务器一直在做什么？ <br/>
A: Watch for inbound connections on the port its listening and accept them.

Q: Calling [accept](http://man.cx/accept%282%29 accept) yields a descriptor. What do I do with it?<br/>
A: Save the descriptor and do a non-blocking read/write operation on it.

Q: Why does the read/write have to be non-blocking?<br/>
A: If the file operation ( even a socket in Unix is a file ) is blocking how could the server for example accept other connection requests when its blocked in a file I/O operation.

Q: I guess I have to do many such non-blocking operations on the socket to see when it's ready. Am I right?<br/>
A: Yes. That is what an event library does for you. Now you get it.

Q: How do Event Libraries do what they do?<br/>
A: They use the operating system's [polling](http://www.devshed.com/c/a/BrainDump/Linux-Files-and-the-Event-Poll-Interface/) facility along with timers.

Q: So are there any open source event libraries that do what you just described? <br/>
A: Yes. `libevent` and `libev` are two such event libraries that I can recall off the top of my head.

Q: Does Redis use such open source event libraries for handling socket I/O?<br/>
A: No. For various [reasons](http://groups.google.com/group/redis-db/browse_thread/thread/b52814e9ef15b8d0/) Redis uses its own event library.
