Principles of Reliable Systems
==============================

:Authors: Garrett Smith
:Time: 3:30 pm - 4:20 pm
:Session: https://thestrangeloop.com/sessions/lessons-from-erlang-principles-of-reliable-systems

Reliability

This is unsexy like the 1980s Automotive Quality Wars. The lesson from
the quality wars is that things that break suck. But things that keep
going are awesome. Awesome like the Terminator who keeps going after
getting shot in the face with a shotgun. People will spend money for
quality, and they will develop loyalties to things that don't break.
They will avoid things that break (or that are perceived to break).

Reliability (quality) is central to the Erlang community, in large
part due to the [mythic] number of 9 9's of uptime put forward by Joe
Armstrong. Erlang was a commercially motivated language, not
academically motivated, so quality and reliability had an associated
cost for the creators. Erlang came out of PLEX, another language
Ericsson designed. PLEX was a real time, very parallel language, but
it was very low level, and therefore very expensive to use. The idea
was to find or develop a new language that had an OS independent VM
and had great support for parallelism and concurrency. Erlang was the
product of this, and because of its motivations it prizes pragmatism
over purity.

Principles of Reliability

* Isolation
* Fault detection and Recovery
* Separation of concerns
* Black box design
* State management
* Avoid complexity

Isolation
---------

The Erlang VM is designed to operate processes. You can kill
individual Erlang processes without impacting other processes on the
VM. We see isolation all around us in the physical world, sort of by
definition. So we need to think about how to apply the same ideas to
software and think about how to keep things isolation from one
another.

Fault Detection and Recovery
----------------------------

In order to recover from a failure, you need to be able to detect the
failure first. This isn't as easy as it sounds, especially at the
thread level. When thinking about how to detect failure, you want to
detect this as quickly as possible to "fail fast". Once the failure
has been detected, you need a strategy to recovery. Erlang addresses
this by "turning it off and on again" -- restarting the piece of code
that registered the failure.

Separation of Concerns
----------------------

Separation of Concerns is the principle of focusing on one thing, and
doing it well. [Cohesion, etc.] By keeping code focused, it's easier
to reason about it, test it, and limit the scope for a change. This
also means that if something fails, the scope of failure is limited.

Black Box Design
----------------

Black box design is an approach to designing things where you treat
your components as an appliance. The appliances in your home may be
quite complex (washer, microwave, etc), but the interface it presents
is limited by design. Thinking about code as an appliance means you
try to make it easy to set up (just plug it in?), push the start
button, provide minimal controls, and reboot or replace to fix.

Erlang is effectively an "operating system" for your code. So you
write "systems", and individual "programs" within that service handle
some specific concern.

State Management
----------------

Erlang doesn't "hate" state, but it doesn't like it very much. Messing
with data is costly, and as soon as state enters your application you
have to deal with additional complexity during recovery, failover,
repair, and synchronization. All of these are hard to get right. If
you can avoid state, you should, either by avoiding it completely, or
ensuring it's someone else's problem.

Avoid Complexity
----------------

Reducing complexity means there's fewer edges to test. Things like
dependencies, hierarchies, resource sharing, and fear all are
indicators of complexity. Something simple is something reliable. And
if something isn't completely obvious, spend some time making sure
someone else could understand it.

How to Do This
--------------

OS Process Isolation
~~~~~~~~~~~~~~~~~~~~

* No shared memory
* Communicate via message passing
* Process termination ("fault") detection
* Techniques: IO "servers", 0MQ, TCP/HTTP

Actors
~~~~~~

* Processes have overhead, and at some scales aren't feasible
* Actors provide semantically isolated memory
* Inter-thread communication via message passing (queue inserts)

Fail Fast
~~~~~~~~~

* Avoid defensive practices -- let things fail
* Let exceptions propagate and log them
* Use assertions -- and leave them in!
* Exiting the process isn't a bad idea if you're running under a
  supervisor
* runit, launchd provide process monitoring/supervision [speaker
  recommends runit]

Think Small
~~~~~~~~~~~

* Narrow the scope as much as possible
* Aim for functional-style programming -- average functions are four
  lines long
* Think about a Micro SOA
* Avoid building for "the future"
