---
layout: post
title: "Java EE 6 Web Profile Without the App Server. Part 4."
date: 2012-07-08 08:34
comments: true
categories: [Java, CDI, JPA]
---
The story so far
----------------
In [part 1](http://www.lifeasageek.com/2012/06/java-ee-6-web-profile-without-container.html) we covered getting our SE app started and the Jetty and Weld containers started to provide us with a servlet and CDI capabilities. In [part 2](http://www.lifeasageek.com/2012/06/java-ee-6-web-profile-without-app.html) we added support for RESTful webservices with the Jersey implementation of JAX-RS. In [part 3](http://www.lifeasageek.com/2012/06/java-ee-6-web-profile-without-app_26.html) we added JSF support using Mojarra 2.1.

This time we're going to be adding JPA 2 support using Hibernate we will also be making our EntityManager injectable and implemented container transations using our own CDI interceptor.

<font color="green">As always a fully working project to go along with this post is available [here](https://github.com/justinwyer/goodstuff-example/tree/part4).</font>

JPA 2
-----
JPA 2 is probably one of the best JCP and JSR success stories. I really love this specification and every implementation I have used has done a really great job.

The implementation I have chosen to use is Hibernate but you can switch this out if you wish!

We're going to implement our own EntityManager which will simply wrap a delegate, this gives us two things:

1.   An EntityManager we can **@Inject**
2.   It gives us fine grained control of the lifecycle of our delegate EntityManager.

We will also create an **@ApplicationScoped** CDI producer which will produce the delegate for our wrapper.

Lastly we will be creating a transaction interceptor to automatically begin/commit/rollback our transactions.

We add hibernate and h2 database as dependencies.
{% gist 3071648 pom.xml %}
Next we setup our persistence.xml
{% gist 3071648 persistence.xml %}
And our entity
{% gist 3071648 Deed.java %}

Now for the exciting stuff first up the producer
{% gist 3071648 EntityManagerProducer.java %}
The producer looks after our delegate EntityManager lifecycle, creating a new one when required and more importantly allowing the TransactionInterceptor to destroy it when a transaction ends.

Next we implement our EntityManager to wrap our delegate, because we are in a CDI container the our wrapper also lets us **@Inject** a **@Dependent** scoped EntityManager into any CDI bean.
{% gist 3071648 EntityManagerWrapper.java %}
As you see the wrapper is very straight forward, but with a CDI container at work the wrapper plays an important role.

The final piece of the puzzle is our TransactionInterceptor, it knits our wrapper and delegate producer together and makes our JPA transactions automatic.
{% gist 3071648 TransactionInterceptor.java %}
The logic is fairly straight forward the important part is that we unregister the delegate with our producer when a transaction goes out of scope.

We also need the interceptor binding...
{% gist 3071648 Transactional.java %}
... and remember to enable our interceptor in the beans.xml
{% gist 3071648 beans.xml %}
We can now **@Inject** our EntityManager and transactions are taken care of automatically if we annotate our bean or method **@Transactional**.

Thats it for now in this series of blog posts. You can fork the [master](https://github.com/justinwyer/goodstuff-example) branch of the example project and have a solid start for your application. If you have any ideas for more topics in this series or questions about the topics covered so far let me know on Twitter or Google+!
