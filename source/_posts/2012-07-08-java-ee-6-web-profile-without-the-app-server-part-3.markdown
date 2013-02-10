---
layout: post
title: "Java EE 6 Web Profile without the app server. Part 3."
date: 2012-06-26 09:56
comments: true
categories: [Java, CDI, JSF]
permalink: /2012/06/java-ee-6-web-profile-without-app_26.html
---
The story so far
----------------
In [part 1](http://www.lifeasageek.com/2012/06/java-ee-6-web-profile-without-container.html) we covered getting our SE app started and the Jetty and Weld containers started to provide us with a servlet and CDI capabilities. In [part 2](http://www.lifeasageek.com/2012/06/java-ee-6-web-profile-without-app.html) we added support for RESTful webservices with the Jersey implementation of JAX-RS.

Now its time for some UI, for this we're going to setup setup JSF  using the Mojarra 2.1 implementation.

<font color="green">As always a fully working project to go along with this post is available [here](https://github.com/justinwyer/goodstuff-example/tree/part3).</font>
Java Server Faces
-----------------
Lots of people love JSF and a lot of people don't. In general I am not a frontend hacker, and to be honest when I have to do it (and I am usually forced to use a UI framework) I find myself wishing for plain old HTML or that new cool term [POH5](http://vimeo.com/33538130). Now whether this is the fault of Java, the UI framework or **GASP** a failing deep within myself I am not sure.

My experience is that JSF makes writing web applications in Java feel the least like trying to parallel park an [articulated bus](http://en.wikipedia.org/wiki/Articulated_bus).

Remember the real problem with web development is that some  always mentions wanting support for some version of Internet Explorer ;)

So all of that said how do we go about doing this? Well its really pretty simple at this point. We have most of the environment in place already, all we need now are a few extra dependencies, a bit in the web.xml and a faces-config.xml.
{% gist 3070798 pom.xml %}
The four dependencies that we need are the Mojarra JSF API and implementation, JSTL and lastly a EL implementation (we added the API in part 2).
{% gist 3070798 web.xml %}
Above we ask Mojarra to configure JSF for us and we setup the EL implementation.

If you check out the example there is also a DeedController bean, and an index.xhtml you can build and run the example and point your browser at [http://localhost:8080/faces/index.xhtml](http://localhost:8080/faces/index.xhtml).

In part 4 we'll setup JPA and using CDI make our EntityManager injectable and setup container transactions.
