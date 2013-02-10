---
layout: post
title: "Java EE 6 Web Profile without the app server. Part 1."
date: 2012-06-21 14:24
comments: true
categories: [Java, CDI]
permalink: /2012/06/java-ee-6-web-profile-without-container.html
---
What is the problem?
--------------------
You want to write a Java app, that has all the good stuff in it?

By good stuff I mean:

*   CDI (who doesn't want awesome DI and interceptors) 
*   JAX RS 
*   JSF 
*   JPA 2.0 (with container managed transactions) 

Of course you do, why wouldn't you? I love an app server as much as the next guy or gal, but it is another piece of the stack which needs to be monitored and patched, it can also have its own bag of issues you need to look out for.

So we recently wrote a product and I wanted to use all the "good stuff". Since the product is to be widely deployed I veto'd the use of an app server after all less moving parts means less support calls.

So I went about getting all these goodies working in Java SE and here is how I did it.

<font color="green">A fully working project to go along with this post is available [here](https://github.com/justinwyer/goodstuff-example/tree/part1).</font>

Servlet Container
-----------------
You're going to need one of these, I picked Jetty 8 its really easy to get running in embedded mode and it has JNDI. That said you could probably use Tomcat 7 without too much modification.
{% gist 3070683 App.java %}
The above is some pretty standard stuff to get embedded jetty going.
{% gist 3070683 web.xml %}
CDI Container
-------------
You'll also be needing one of these, I chose Weld for the job. Your web.xml would need to look something like the snippet below.
{% gist 3070683 App.java %}
The Weld servlet listener will start our CDI container and the init() method of AppServlet is our entry point into our application after the containers have loaded, so you can go ahead and put any initialization you want in here with the CDI container available!
{% gist 3070683 AppServlet.java %}
Next we have our CDI bean.
{% gist 3070683 GoodStuff.java %}
Our CDI interceptor implementation...
{% gist 3070683 GoodInterceptor.java %}
... and the interceptor annotation.
{% gist 3070683 Good.java %}
We even have an Arquillian unit test.
{% gist 3070683 AppTest.java %}
In [part 2](http://www.lifeasageek.com/2012/06/java-ee-6-web-profile-without-app.html) we will get JAX RS working as well.
