---
layout: post
title: "Java EE 6 Web Profile without the app server. Part 2."
date: 2012-06-22 12:35
comments: true
categories: [Java, CDI, JAX-RS]
permalink: /2012/06/java-ee-6-web-profile-without-app.html
---

The story so far
----------------
In [part 1](http://www.lifeasageek.com/2012/06/java-ee-6-web-profile-without-container.html) we covered getting our SE app started and the Jetty and Weld containers started to provide us with a servlet and CDI capabilities. Now we are going to setup JAX RS.

RESTful services
----------------
Do you want to add restful services to our application? Of course you do!

<font color="green">A fully working project to go along with this post is available [here](https://github.com/justinwyer/goodstuff-example/tree/part2).</font>

JAX RS Provider
---------------
I chose Jersey however it should be just as easy to get RESTeasy integrated. First off we need to tell Jetty about Jersey.
{% gist 3070769 web.xml %}
So in the above we setup our Jersey servlet and we set the javax.ws.rs.Application parameter, the value is a class that we need to implement which returns a list of all the JAX RS resources in our application.
{% gist 3070769 RestConfig.java %}
Our Service
-----------
Its also good idea to actually write the RESTful service.
{% gist 3070769 DeedService.java %}
So go ahead and POST to [http://localhost:8080/rest/deed/good](http://localhost:8080/rest/deed/good) and you should receive a 201 Created response

You can also see the CDI lifecycle taking place on stdout.

In [part 3](http://www.lifeasageek.com/2012/06/java-ee-6-web-profile-without-app_26.html) we will get JSF 2.0 working as well.
