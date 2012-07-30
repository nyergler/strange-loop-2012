Engineering Elegance: The Secrets of Square's Stack
===================================================

:Authors: Bob Lee
:Time: 4:30 pm - 5:20 pm
:Session: https://thestrangeloop.com/sessions/engineering-elegance-the-secrets-of-squares-stack

@crazybob

As CTO of Square, solely focused on technology, not managing people.

Persistent Queues
-----------------

Card processing has two phases:

#. Authorization
   Sets aside the funds for the merchant.
#. Capture
   "Commit"

This actually works pretty well for mobile: you enter the amount and
the authorization occurs. After the signature screen, you can capture
(commit). Since the mobile network is inherently faulty, capture
occurs in the "background" -- capture is queued, and it will
eventually be handled.

This gives the feeling of responsiveness, but this is obviously
critical code, so what happens if it fails? Their answer is
*persistent queues*. SQLite was an open, but felt like a mismatch for
building a queue (b-tree vs fifo, etc).

Needed atomicity and durability. Began by investigating what you get
from the filesystem. Renaming is atomic, fsync is durable (unless your
hardware lies to you), but there were questions about whether or
not segment/block writes are atomic. After investigating, it appears
that you can rely on them being atomic.

There are a few traditional strategies for implementing this sort of
application:

* write / fsync / rename / fsync (note you need both fsyncs to avoid
  operation re-ordering where you'd wind up with an empty file)
* rollback log
* journal

Solving a specific use case (FIFO queues), so built and open sourced
Tape (http://github.com/square/tape/) to accomplish this.

Server Stack
------------

Original stack written using Ruby, but ran into scalability problems.
Ruby requires one process per request, and isn't great about sharing
memory. This limits you to roughly 20-25 request workers per machine,
which isn't great for a high volume transaction processing.

Tried using JRuby to take advantage of better concurrency support, but
many of the underlying libraries weren't built for concurrency. All
the core payment processing code now uses Java.

The port to Java began about 1.5 years ago. Bob spent the previous
five years at Google, who has their own proprietary JVM stack. In
addition, Bob had been on Android for 3 years, so he was out of touch
with the latest and greatest. What he found:

* One Repository for all the Java code

  * Don't version internal dependencies, everything builds against
    master
  * This means that if you want to change something low level, you
    also have to fix everything that uses it

* One JAR for deploying the application

  * Hot deployment of WAR files often has problems with memory leaks
  * Instead of an application server, they use a single JAR that has a
    ``main`` method that runs an embedded server.
  * Typical way to do this is unpacking and repacking into a single
    JAR.
  * OneJAR (available on SourceForge) actually allows you to do nested
    JARs. This speeds up the build and delays when you run into the
    64000 file limit per JAR
  * This does mean you can't do classpath scanning (but you probably
    shouldn't be anyway).
  * You can also do self-executing JARs

* Java Stack

  * Jetty
  * JAX-RS (Jersey) -- higher level abstraction over Servlets
  * JPA (Hibernate)
  * Guice
  * *Dropwizard* -- they don't use it, but probably going to migrate
    there.


Rethinking Publish / Subscribe
------------------------------

Lots of messages to pass around; i.e., payment processing needs to
tell a capture system, a settlement system, risk systems, etc about
things that happen. The standard approach would be to use a messaging
server for that, with reliability implemented with additional
messaging servers. The producer and consumer are probably already HA
systems, so this adds another cluster to deploy and maintain.

Instead of messaging, they're using a message feed based system.

#. Client asks for all records
#. Server responds with the records and *current version*
#. Later, Client asks for the delta since the last version it saw
#. Server responds with delta

Requires:

* Immutable sequence of events
* Total ordering
* Centralized server

Benefits:

* Stateless
* Fewer moving parts
* Bootstrapping for free

Further ideas:

* Partitioning feeds
* Caching and replicating feeds
* PubSubHubBub

Dependency Injection
--------------------

Guice is a dependency injection container developed at Google in 2006.

Guice uses a DSL written in Java, which means that for a tool to
understand your Guice configuration, it needs to execute your module.

If they could go back and do it again, maybe code generation would be
a better way to go than a DSL. Java has an annotation processing API
(JSR-XXX), which Bob served on the expert group for, which hasn't
gained much adoption.

Guice also has provider methods, which if they had existed in Guice 1,
might have obviated the need for the binder API.


Developed Dagger (http://github.com/square/dagger), which applies the
learnings from Guice for better dependency injection. Use of code
generation means that many errors are compiler errors instead of run
time errors. Resulted in increased startup speed for their Android
applications.

Engineering blog: http://corner.squareup.com
