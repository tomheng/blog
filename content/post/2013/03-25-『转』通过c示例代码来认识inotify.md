---
date: Mon, 25 Mar 2013 14:12:16 +0000
title: 『转』通过C示例代码来认识Inotify
author: tomheng
layout: post
permalink: /archives/1672.html
views: 202
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - Inotify
---
原文：[Inotify Example: Introduction to Inotify with a C Program Example][1]

inotify utility is an effective tool to monitor and notify filesystem changes.  You can specify a list of files and directories that needs to be monitored by inotify. This library is used by various other programs. For example, CPAN module Linux::Inotify is developed based on this library.

### iNotify Execution Flow

On a high-level, you do the following with inotify utility.

  * **Create inotify monitoring list.** Add the desired directories/files to the inotify monitoring list. Monitoring list can be changed as and when needed
  * **Request Inotify to report specific event changes** to the monitoring list of files and directories. For example, request inotify to report ON ACCESS, ON OPEN, ON WRITING, ON CLOSE,etc.,

Following are the inotify functions and their corresponding roles.

  * Create the inotify instance by **inotify_init**().
  * Add all the directories to be monitored to the inotify list using **inotify\_add\_watch**() function.
  * To determine the events occurred, do the **read()** on the inotify instance. This read will get blocked till the change event occurs. It is recommended to perform selective read on this inotify instance using **select()** call.
  * Read returns list of events occurred on the monitored directories. Based on the return value of read(), we will know exactly what kind of changes occurred.
  * In case of removing the watch on directories / files, call **inotify\_rm\_watch()**.

Be careful when using this module with NFS filesystem. It might not determine the events changes to the monitoring list that contains files/directories from the NFS filesystem.

### Inotify Events

Following are the available inotify events:

  * IN_ACCESS – File was accessed
  * IN_ATTRIB – Metadata changed (permissions, timestamps, extended attributes, etc.)
  * IN\_CLOSE\_WRITE – File opened for writing was closed
  * IN\_CLOSE\_NOWRITE – File not opened for writing was closed
  * IN_CREATE – File/directory created in watched directory
  * IN_DELETE – File/directory deleted from watched directory
  * IN\_DELETE\_SELF – Watched file/directory was itself deleted
  * IN_MODIFY – File was modified
  * IN\_MOVE\_SELF – Watched file/directory was itself moved
  * IN\_MOVED\_FROM – File moved out of watched directory
  * IN\_MOVED\_TO – File moved into watched directory
  * IN_OPEN – File was opened

### Recommended modules / libraries for iNotify

Make sure libc6 2.3.6 module is installed on your system. If you have a previous version of libc module installed, you will get the following error message while compiling the inotify monitoring c program.

<pre>error: linux/inotify.h: No such file or directory</pre>

Check the libc6 version on your system and upgrade it if required.

<pre># dpkg -l libc6</pre>

### Sample C program for Monitoring of File/directory changes event

<div class="codecolorer-container c blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />25<br />26<br />27<br />28<br />29<br />30<br />31<br />32<br />33<br />34<br />35<br />36<br />37<br />38<br />39<br />40<br />41<br />42<br />43<br />44<br />45<br />46<br />47<br />48<br />49<br />50<br />51<br />52<br />53<br />54<br />55<br />56<br />57<br />58<br />59<br />60<br />61<br />62<br />63<br />64<br />65<br />
        </div>
      </td>
      
      <td>
        <div class="c codecolorer">
          <span class="coMULTI">/*This is the sample program to notify us for the file creation and file deletion takes place in “/tmp” directory*/</span><br /> <span class="co2">#include <stdio.h></span><br /> <span class="co2">#include <stdlib.h></span><br /> <span class="co2">#include <errno.h></span><br /> <span class="co2">#include <sys/types.h></span><br /> <span class="co2">#include <linux/inotify.h></span><br /> <br /> <span class="co2">#define EVENT_SIZE &nbsp;( sizeof (struct inotify_event) )</span><br /> <span class="co2">#define EVENT_BUF_LEN &nbsp; &nbsp; ( 1024 * ( EVENT_SIZE + 16 ) )</span><br /> <br /> <span class="kw4">int</span> main<span class="br0">&#40;</span> <span class="br0">&#41;</span><br /> <span class="br0">&#123;</span><br /> &nbsp; <span class="kw4">int</span> length<span class="sy0">,</span> i <span class="sy0">=</span> <span class="nu0"></span><span class="sy0">;</span><br /> &nbsp; <span class="kw4">int</span> fd<span class="sy0">;</span><br /> &nbsp; <span class="kw4">int</span> wd<span class="sy0">;</span><br /> &nbsp; <span class="kw4">char</span> buffer<span class="br0">&#91;</span>EVENT_BUF_LEN<span class="br0">&#93;</span><span class="sy0">;</span><br /> <br /> &nbsp; <span class="coMULTI">/*creating the INOTIFY instance*/</span><br /> &nbsp; fd <span class="sy0">=</span> inotify_init<span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> &nbsp; <span class="coMULTI">/*checking for error*/</span><br /> &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> fd <span class="sy0"><</span> <span class="nu0"></span> <span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; <a href="http://www.opengroup.org/onlinepubs/009695399/functions/perror.html"><span class="kw3">perror</span></a><span class="br0">&#40;</span> <span class="st0">"inotify_init"</span> <span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; <span class="br0">&#125;</span><br /> <br /> &nbsp; <span class="coMULTI">/*adding the “/tmp” directory into watch list. Here, the suggestion is to validate the existence of the directory before adding into monitoring list.*/</span><br /> &nbsp; wd <span class="sy0">=</span> inotify_add_watch<span class="br0">&#40;</span> fd<span class="sy0">,</span> <span class="st0">"/tmp"</span><span class="sy0">,</span> IN_CREATE <span class="sy0">|</span> IN_DELETE <span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> &nbsp; <span class="coMULTI">/*read to determine the event change happens on “/tmp” directory. Actually this read blocks until the change event occurs*/</span> <br /> <br /> &nbsp; length <span class="sy0">=</span> read<span class="br0">&#40;</span> fd<span class="sy0">,</span> buffer<span class="sy0">,</span> EVENT_BUF_LEN <span class="br0">&#41;</span><span class="sy0">;</span> <br /> <br /> &nbsp; <span class="coMULTI">/*checking for error*/</span><br /> &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> length <span class="sy0"><</span> <span class="nu0"></span> <span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; <a href="http://www.opengroup.org/onlinepubs/009695399/functions/perror.html"><span class="kw3">perror</span></a><span class="br0">&#40;</span> <span class="st0">"read"</span> <span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; <span class="br0">&#125;</span> &nbsp;<br /> <br /> &nbsp; <span class="coMULTI">/*actually read return the list of change events happens. Here, read the change event one by one and process it accordingly.*/</span><br /> &nbsp; <span class="kw1">while</span> <span class="br0">&#40;</span> i <span class="sy0"><</span> length <span class="br0">&#41;</span> <span class="br0">&#123;</span> &nbsp; &nbsp; <span class="kw4">struct</span> inotify_event <span class="sy0">*</span>event <span class="sy0">=</span> <span class="br0">&#40;</span> <span class="kw4">struct</span> inotify_event <span class="sy0">*</span> <span class="br0">&#41;</span> <span class="sy0">&</span>buffer<span class="br0">&#91;</span> i <span class="br0">&#93;</span><span class="sy0">;</span> &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> event<span class="sy0">-></span>len <span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> event<span class="sy0">-></span>mask <span class="sy0">&</span> IN_CREATE <span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> event<span class="sy0">-></span>mask <span class="sy0">&</span> IN_ISDIR <span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <a href="http://www.opengroup.org/onlinepubs/009695399/functions/printf.html"><span class="kw3">printf</span></a><span class="br0">&#40;</span> <span class="st0">"New directory %s created.<span class="es1">\n</span>"</span><span class="sy0">,</span> event<span class="sy0">-></span>name <span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">else</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <a href="http://www.opengroup.org/onlinepubs/009695399/functions/printf.html"><span class="kw3">printf</span></a><span class="br0">&#40;</span> <span class="st0">"New file %s created.<span class="es1">\n</span>"</span><span class="sy0">,</span> event<span class="sy0">-></span>name <span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span><br /> &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span><br /> &nbsp; &nbsp; &nbsp; <span class="kw1">else</span> <span class="kw1">if</span> <span class="br0">&#40;</span> event<span class="sy0">-></span>mask <span class="sy0">&</span> IN_DELETE <span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> event<span class="sy0">-></span>mask <span class="sy0">&</span> IN_ISDIR <span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <a href="http://www.opengroup.org/onlinepubs/009695399/functions/printf.html"><span class="kw3">printf</span></a><span class="br0">&#40;</span> <span class="st0">"Directory %s deleted.<span class="es1">\n</span>"</span><span class="sy0">,</span> event<span class="sy0">-></span>name <span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">else</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <a href="http://www.opengroup.org/onlinepubs/009695399/functions/printf.html"><span class="kw3">printf</span></a><span class="br0">&#40;</span> <span class="st0">"File %s deleted.<span class="es1">\n</span>"</span><span class="sy0">,</span> event<span class="sy0">-></span>name <span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span><br /> &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span><br /> &nbsp; &nbsp; <span class="br0">&#125;</span><br /> &nbsp; &nbsp; i <span class="sy0">+=</span> EVENT_SIZE <span class="sy0">+</span> event<span class="sy0">-></span>len<span class="sy0">;</span><br /> &nbsp; <span class="br0">&#125;</span><br /> &nbsp; <span class="coMULTI">/*removing the “/tmp” directory from the watch list.*/</span><br /> &nbsp; &nbsp;inotify_rm_watch<span class="br0">&#40;</span> fd<span class="sy0">,</span> wd <span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> &nbsp; <span class="coMULTI">/*closing the INOTIFY instance*/</span><br /> &nbsp; &nbsp;close<span class="br0">&#40;</span> fd <span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> <span class="br0">&#125;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

&#8212;  
此外还有一片中文的补充介绍也值得一读。  
<http://cunsheng.sinaapp.com/?p=334>

 [1]: http://www.thegeekstuff.com/2010/04/inotify-c-program-example/
