=====================================
Eventually Consistent Data Structures
=====================================

:Authors: Sean Cribbs
:Time: 1:00 pm - 1:50 pm
:Session: https://thestrangeloop.com/sessions/eventually-consistent-data-structures
:Link:


Works one Riak, an eventually consistent data store (which some people
may call a database). Voldemort and Cassandra are also eventually
consistent. Riak is not ACID compliant, as we heard yesterday.

We have lots of duels/duals in CS -- OOP v Functional, etc. The
duel/dual of safety vs liveness was defined my Lamport in 1977 in
"Proving the Correctness of Multiprocess Programs". Safety means
"nothing bad happens" (partial correctness), where liveness means
"something good eventually happens" (termination). Forcing or
encouraging one property will reduce the other. Peter Bailis talked
about this in his `blog post`_, "Safety and liveness: Eventual
consistency is not safe". It's not safe *by itself*.

With Eventual Consistency, you have multiple independent actors who
are replicating data amongst themselves with loose coordination (for
both reading and writing). They also have convergence -- moving
towards a single shared state. If you don't have convergence, you
don't necessarily have *inconsistency*, but you definitely don't have
eventual consistency. Unlike ACID systems, EC systems do not have
total ordering of events. [This is a problem for some people.]

So what do you do about consistency when there's no clear winner?
Throw one out? Keep both? Cassandra throws one out, Riak and Voldemort
raises conflicts ("siblings" in Riak). So what do you do in this
state? Semantic Resolution -- using domain specific business rules to
resolve -- is the most obvious approach, but in practice it can be
really hard.

"Ad hoc approaches have proven brittle and error prone."

Conflict Free Replicated Data Types
===================================

Instead of opaque data types/blobs in your data store you have useful
abstractions. And because we're in a replicated environment, you have
multiple independent copies. They're conflict free because they
resolve automatically toward a single value. Described in the paper
"Logic and Lattices for Distributed Programming". These structures are
rooted in the theory of monotonic logic.

Bounded Join Semi-Lattices

::

  <S, f, t>

S is a set -- possibly unbounded -- of all possible values. t is a
member of S [less than all other values?]. And f is a function describing
the least-upper bound (join/merge) on S. This provides a partial
ordering for the values of S.

[ Slides show *lset* and *lmax* lattices ]

Lattices give us determinism in how we merge our conflicts -- there is
only one way to merge.

Another paper, "A comprehensive study of Convergent and Commutative
Replicated Data Types", also provides some excellent information.

Two flavors of CRDTs:

* Convergent

  The data you're transmitting is the *state*; weak messaging
  requirements

* Commutative

  The data you're transmitting describe operations. This requires
  reliable broadcast, and causal ordering is sufficient.

Registers
---------

A place to put yourself.

Concurrent updates to this type do not commute, so who wins? The two
strategies are the basic strategies used by Cassandra/Riak. Last Write
Wins (LWW-Register) used by Cassandra, Multi-Valued (MV-Register) used
by Riak.

Counters
--------

Replicated integers with two operations: increment and decrement. An
operation based counter does not depend on delivery order (since
addition is commutative).

G-Counter is a Grow Only Counter, with a minimum value of 0. You keep
track of how each member of the cluster has counted.

PN-Coutners are similar, but you can go positive or negative. Again,
you keep track of state for each member, and use a function to derive
the actual value and resolve conflicts.

Sets
----

G-Set describes a set that can only be added to.

2P-Set (two phase set) describes a set where once something is removed
from the set, it can not be re-added. Two G-Sets composed into a
single type. One set describes the additions, the other the removals.
The "value" is the difference of the two sets.

U-Set -- every value has a tag that indicates uniqueness

OR-Set (Observed Remove set)

Graphs
------

"Unfortunately they're really complicated and error prone." :)

Working with Graphs in a distributed environment you can run into
problems when two simultaneous additions create a cycle, potentially
violating a global invariant.

Use Cases
=========

* Social graph -- OR Set
* Web page visits -- G Counter
* Shopping Cart -- Modified OR Set
* "Like" button -- U-Set (handles lots of concurrent writes)

Challenges
==========

CRDTs are often inefficient, presenting a challenge for garbage
collection (which may require synchronization).

It's also not clear who's responsible for the synchronization. Some
client libraries implement this -- mochi/statebox (Erlang),
reiddraper/knockbox (Clojure), etc -- but the clients aren't
participating in replication, so there's some possible inefficiencies
and additional garbage created.

Riak will be implementing support for these on the server side.



.. _`blog post`: http://www.bailis.org/blog/safety-and-liveness-eventual-consistency-is-not-safe/
