---
date: Sat, 13 Jun 2015 15:06:27 +0000
title: '[转]Inside NGINX: How We Designed for Performance &#038; Scale'
author: tomheng
layout: post
permalink: /archives/1938.html
views: 112
categories:
  - LAMP
tags:
  - Nginx
---
NGINX leads the pack in web performance, and it’s all due to the way the software is designed. Whereas many web servers and application servers use a simple threaded or process-based architecture, NGINX stands out with a sophisticated event-driven architecture that enables it to scale to hundreds of thousands of concurrent connections on modern hardware.

The [Inside NGINX][1] infographic drills down from the high-level process architecture to illustrate how NGINX handles multiple connections within a single process. This blog explains how it all works in further detail.

## Setting the Scene – the NGINX Process Model

<img class="aligncenter size-full wp-image-1939" src="http://blog.webfuns.net/wp-content/uploads/2015/06/Screen-Shot-2015-06-08-at-12.36.30-PM.png" alt="Screen-Shot-2015-06-08-at-12.36.30-PM" width="740" height="439" />

To better understand this design, you need to understand how NGINX runs. NGINX has a master process (which performs the privileged operations such as reading configuration and binding to ports) and a number of worker and helper processes.

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="co0"># service nginx restart</span><br /> <span class="sy0">*</span> Restarting nginx<br /> <span class="co0"># ps -ef --forest | grep nginx</span><br /> root <span class="nu0">32475</span> <span class="nu0">1</span> <span class="nu0"></span> <span class="nu0">13</span>:<span class="nu0">36</span> ? 00:00:00 nginx: master process <span class="sy0">/</span>usr<span class="sy0">/</span>sbin<span class="sy0">/</span>nginx \<br /> <span class="re5">-c</span> <span class="sy0">/</span>etc<span class="sy0">/</span>nginx<span class="sy0">/</span>nginx.conf<br /> nginx <span class="nu0">32476</span> <span class="nu0">32475</span> <span class="nu0"></span> <span class="nu0">13</span>:<span class="nu0">36</span> ? 00:00:00 \_ nginx: worker process<br /> nginx <span class="nu0">32477</span> <span class="nu0">32475</span> <span class="nu0"></span> <span class="nu0">13</span>:<span class="nu0">36</span> ? 00:00:00 \_ nginx: worker process<br /> nginx <span class="nu0">32479</span> <span class="nu0">32475</span> <span class="nu0"></span> <span class="nu0">13</span>:<span class="nu0">36</span> ? 00:00:00 \_ nginx: worker process<br /> nginx <span class="nu0">32480</span> <span class="nu0">32475</span> <span class="nu0"></span> <span class="nu0">13</span>:<span class="nu0">36</span> ? 00:00:00 \_ nginx: worker process<br /> nginx <span class="nu0">32481</span> <span class="nu0">32475</span> <span class="nu0"></span> <span class="nu0">13</span>:<span class="nu0">36</span> ? 00:00:00 \_ nginx: cache manager process<br /> nginx <span class="nu0">32482</span> <span class="nu0">32475</span> <span class="nu0"></span> <span class="nu0">13</span>:<span class="nu0">36</span> ? 00:00:00 \_ nginx: cache loader process
        </div>
      </td>
    </tr>
  </table>
</div>

On this 4-core server, the NGINX master process creates 4 worker processes and a couple of cache helper processes which manage the on-disk content cache.

## Why Is Architecture Important?

The fundamental basis of any Unix application is the thread or process. (From the Linux OS perspective, threads and processes are mostly identical; the major difference is the degree to which they share memory.) A thread or process is a self-contained set of instructions that the operating system can schedule to run on a CPU core. Most complex applications run multiple threads or processes in parallel for two reasons:

  * They can use more compute cores at the same time.
  * Threads and processes make it very easy to do operations in parallel (for example, to handle multiple connections at the same time).

Processes and threads consume resources. They each use memory and other OS resources, and they need to be swapped on and off the cores (an operation called a *context switch*). Most modern servers can handle hundreds of small, active threads or processes simultaneously, but performance degrades seriously once memory is exhausted or when high I/O load causes a large volume of context switches.

The common way to design network applications is to assign a thread or process to each connection. This architecture is simple and easy to implement, but it does not scale when the application needs to handle thousands of simultaneous connections.

## How Does NGINX Work?

NGINX uses a predictable process model that is tuned to the available hardware resources:

  * The *master* process performs the privileged operations such as reading configuration and binding to ports, and then creates a small number of child processes (the next three types).
  * The *cache loader* process runs at startup to load the disk-based cache into memory, and then exits. It is scheduled conservatively, so its resource demands are low.
  * The *cache manager* process runs periodically and prunes entries from the disk caches to keep them within the configured sizes.
  * The *worker* processes do all of the work! They handle network connections, read and write content to disk, and communicate with upstream servers.

The NGINX configuration recommended in most cases – running one worker process per CPU core – makes the most efficient use of hardware resources. You configure it by including the <a href="http://nginx.org/en/docs/ngx_core_module.html#worker_processes" target="_blank">worker_processes auto</a> directive in the configuration:

> worker_processes auto;

When an NGINX server is active, only the worker processes are busy. Each worker process handles multiple connections in a non-blocking fashion, reducing the number of context switches.

Each worker process is single-threaded and runs independently, grabbing new connections and processing them. The processes can communicate using shared memory for shared cache data, session persistence data, and other shared resources.

## Inside the NGINX Worker Process

&nbsp;

<img class="aligncenter size-full wp-image-1940" src="http://blog.webfuns.net/wp-content/uploads/2015/06/Screen-Shot-2015-06-08-at-12.39.48-PM.png" alt="Screen-Shot-2015-06-08-at-12.39.48-PM" width="866" height="405" />

Each NGINX worker process is initialized with the NGINX configuration and is provided with a set of listen sockets by the master process.

The NGINX worker processes begin by waiting for events on the listen sockets (<a href="http://nginx.org/en/docs/ngx_core_module.html#accept_mutex" target="_blank">accept_mutex</a> and <a href="http://nginx.com/blog/socket-sharding-nginx-release-1-9-1/" target="_blank">kernel socket sharding</a>). Events are initiated by new incoming connections. These connections are assigned to a *state machine* – the HTTP state machine is the most commonly used, but NGINX also implements state machines for stream (raw TCP) traffic and for a number of mail protocols (SMTP, IMAP, and POP3).

<img class="aligncenter size-full wp-image-1941" src="http://blog.webfuns.net/wp-content/uploads/2015/06/Screen-Shot-2015-06-08-at-12.40.32-PM.png" alt="Screen-Shot-2015-06-08-at-12.40.32-PM" width="703" height="742" />

The state machine is essentially the set of instructions that tell NGINX how to process a request. Most web servers that perform the same functions as NGINX use a similar state machine – the difference lies in the implementation.

## Scheduling the State Machine

Think of the state machine like the rules for chess. Each HTTP transaction is a chess game. On one side of the chessboard is the web server – a grandmaster who can make decisions very quickly. On the other side is the remote client – the web browser that is accessing the site or application over a relatively slow network.

However, the rules of the game can be very complicated. For example, the web server might need to communicate with other parties (proxying to an upstream application) or talk to an authentication server. Third-party modules in the web server can even extend the rules of the game.

### A Blocking State Machine

Recall our description of a process or thread as a self-contained set of instructions that the operating system can schedule to run on a CPU core. Most web servers and web applications use a process-per-connection orthread-per-connection model to play the chess game. Each process or thread contains the instructions to play one game through to the end. During the time the process is run by the server, it spends most of its time ‘blocked’ – waiting for the client to complete its next move.

<img class="aligncenter size-full wp-image-1942" src="http://blog.webfuns.net/wp-content/uploads/2015/06/Screen-Shot-2015-06-08-at-12.40.52-PM.png" alt="Screen-Shot-2015-06-08-at-12.40.52-PM" width="373" height="341" />

  1. The web server process listens for new connections (new games initiated by clients) on the listen sockets.
  2. When it gets a new game, it plays that game, blocking after each move to wait for the client’s response.
  3. Once the game completes, the web server process might wait to see if the client wants to start a new game (this corresponds to a keepalive connection). If the connection is closed (the client goes away or a timeout occurs), the web server process returns to listening for new games.

The important point to remember is that every active HTTP connection (every chess game) requires a dedicated process or thread (a grandmaster). This architecture is simple and easy to extend with third-party modules (‘new rules’). However, there’s a huge imbalance: the rather lightweight HTTP connection, represented by a file descriptor and a small amount of memory, maps to a separate thread or process, a very heavyweight operating system object. It’s a programming convenience, but it’s massively wasteful.

### NGINX is a True Grandmaster

Perhaps you’ve heard of <a href="http://en.wikipedia.org/wiki/Simultaneous_exhibition" target="_blank">simultaneous exhibition</a> games, where one chess grandmaster plays dozens of opponents at the same time?

<div id="attachment_1943" style="width: 490px" class="wp-caption aligncenter">
  <img class="wp-image-1943 size-full" src="http://blog.webfuns.net/wp-content/uploads/2015/06/Kiril-Georgiev.gif" alt="Kiril-Georgiev" width="480" height="337" />
  
  <p class="wp-caption-text">
    Kiril Georgiev played 360 people simultaneously in Sofia, Bulgaria. His final score was 284 wins, 70 draws and 6 losses.
  </p>
</div>

That’s how an NGINX worker process plays “chess.” Each worker (remember – there’s usually one worker for each CPU core) is a grandmaster that can play hundreds (in fact, hundreds of thousands) of games simultaneously.

&nbsp;

<img class="aligncenter size-full wp-image-1944" src="http://blog.webfuns.net/wp-content/uploads/2015/06/Screen-Shot-2015-06-08-at-12.41.13-PM.png" alt="Screen-Shot-2015-06-08-at-12.41.13-PM" width="376" height="355" />

  1. The worker waits for events on the listen and connection sockets.
  2. Events occur on the sockets and the worker handles them: 
      * An event on the listen socket means that a client has started a new chess game. The worker creates a new connection socket.
      * An event on a connection socket means that the client has made a new move. The worker responds promptly.

A worker never blocks on network traffic, waiting for its “opponent” (the client) to respond. When it has made its move, the worker immediately proceeds to other games where moves are waiting to be processed, or welcomes new players in the door.

###  Why Is This Faster than a Blocking, Multi-Process Architecture?

NGINX scales very well to support hundreds of thousands of connections per worker process. Each new connection creates another file descriptor and consumes a small amount of additional memory in the worker process. There is very little additional overhead per connection. NGINX processes can remain pinned to CPUs. Context switches are relatively infrequent and occur when there is no work to be done.

In the blocking, connection-per-process approach, each connection requires a large amount of additional resources and overhead, and context switches (swapping from one process to another) are very frequent.

For a more detailed explanation, check out this <a href="http://www.aosabook.org/en/nginx.html" target="_blank">article</a> about NGINX architecture, by Andrew Alexeev, VP of Corporate Development and Co-Founder at NGINX, Inc.

With appropriate [system tuning][2], NGINX can scale to handle hundreds of thousands of concurrent HTTP connections per worker process, and can absorb traffic spikes (an influx of new games) without missing a beat.

## Updating Configuration and Upgrading NGINX

NGINX’s process architecture, with a small number of worker processes, makes for very efficient updating of the configuration and even the NGINX binary itself.

&nbsp;

<img class="aligncenter size-full wp-image-1945" src="http://blog.webfuns.net/wp-content/uploads/2015/06/Screen-Shot-2015-06-08-at-12.41.33-PM.png" alt="Screen-Shot-2015-06-08-at-12.41.33-PM" width="729" height="305" />

Updating NGINX configuration is a very simple, lightweight, and reliable operation. It typically just means running the

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          nginx –s reload
        </div>
      </td>
    </tr>
  </table>
</div>

command, which checks the configuration on disk and sends the master process a SIGHUP signal.

When the master process receives a SIGHUP, it does two things:

  1. Reloads the configuration and forks a new set of worker processes. These new worker processes immediately begin accepting connections and processing traffic (using the new configuration settings).
  2. Signals the old worker processes to gracefully exit. The worker processes stop accepting new connections. As soon as each current HTTP request completes, the worker process cleanly shuts down the connection (that is, there are no lingering keepalives). Once all connections are closed, the worker processes exit.

This reload process can cause a small spike in CPU and memory usage, but it’s generally imperceptible compared to the resource load from active connections. You can reload the configuration multiple times per second (and many NGINX users do exactly that). Very rarely, issues arise when there are many generations of NGINX worker processes waiting for connections to close, but even those are quickly resolved.

NGINX’s binary upgrade process achieves the holy grail of high-availability – you can upgrade the software on the fly, without any dropped connections, downtime, or interruption in service.

<img class="aligncenter size-full wp-image-1946" src="http://blog.webfuns.net/wp-content/uploads/2015/06/Screen-Shot-2015-06-08-at-12.41.51-PM.png" alt="Screen-Shot-2015-06-08-at-12.41.51-PM" width="774" height="284" />

The binary upgrade process is similar in approach to the graceful reload of configuration. A new NGINX master process runs in parallel with the original master process, and they share the listening sockets. Both processes are active, and their respective worker processes handle traffic. You can then signal the old master and its workers to gracefully exit.

The entire process is described in more detail in <a href="http://nginx.org/en/docs/control.html" target="_blank">Controlling NGINX</a>.

## Conclusion

The [Inside NGINX infographic][1] provides a high-level overview of how NGINX functions, but behind this simple explanation is over ten years of innovation and optimization that enable NGINX to deliver the best possible performance on a wide range of hardware while maintaining the security and reliability that modern web applications require.

If you’d like to read more about the optimizations in NGINX, check out these great resources:

  * [Installing and Tuning NGINX for Performance][3] (webinar; <a href="https://speakerdeck.com/nginx/nginx-installation-and-tuning" target="_blank">slides</a> at Speaker Deck)
  * [Tuning NGINX for Performance][2]
  * <a href="http://www.aosabook.org/en/nginx.html" target="_blank">The Architecture of Open Source Applications – NGINX</a>
  * [Socket Sharding in NGINX Release 1.9.1][4] (using the SO_REUSEPORT socket option)

原文：http://nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/

 [1]: http://nginx.com/resources/library/infographic-inside-nginx/
 [2]: http://nginx.com/blog/tuning-nginx/
 [3]: http://nginx.com/resources/webinars/installing-tuning-nginx/
 [4]: http://nginx.com/blog/socket-sharding-nginx-release-1-9-1/
