In Memory Databases
===================

:Authors: Michael Stonebraker
:Time: 9:00 am - 9:50 am
:Session: https://thestrangeloop.com/sessions/in-memory-databases-the-future-is-now
:Link: http://voltdb.com/

VoltDB: Not Your Father's Transaction Processing

"It's a pleasure to be here, because I can look out and there's no one
in the audience in a suit."

The buzzword du jour in research is "big data". The standard marketing
around big data is that I have too much, it's coming too fast, or from
too many places. He's focusing on the aspect of "too fast", and high
velocity ingest.

Transaction processing 30 years ago was highly intermediated. 1000
transactions per second was considered a stretch goal (HPTS 1985). In
this traditional world, ACID was the gold standard, and the workload
was a mix of updates and queries. This was the bread and butter of
Ingres and Oracle.

In the last 25 years, the intermediaries have been removed, which has
led to a cooresponding increase in transaction volume. Transaction
processing now encompasses much more than just data processing; it
includes things like multiplayer games, social networking, ads, etc.
Retaining someone's state in a multiplayer game is a huge transaction
processing problem. Ad placement isn't just a TP problem, it's a
real-time TP problem.

In addition to disintermediation, the rise of sensor tagging
(marathons, taxis, etc) is also adding to the volume of transactions.

High velocity ingest (really anything upstream from Hadoop) adds to
the volume, as well.

But the workload still looks about the same: a mix of queries and
updates, ACID required, but at two orders of magnitude the velocity
and volume.

Reality Check:

TP databases grow in size at the rate transactions
increase: everything you see at Amazon before you click "Buy" is non
TP. For *most* people, 1TB is a really big transaction processing
database. You can buy a terabyte of memory for about $50k (say, 65Gb x
16). Moore's Law has eclipsed TP databases, so it's possible to keep
your database in main memory.

Instrumenting the Shore DBMS prototype to understand performance, only
about 4% of work is actually useful work: buffer pools, locking,
latching, and recovery take the rest of the time.

To go faster than traditional systems, you need to focus on overhead
and get rid of *all* major sources of overhead. If you focus on better
B-trees, this only impacts about 4% of the path length. So don't
bother focusing on the actual transaction. The real gains *must* come
from focusing on overhead.

So why give up on SQL to use a NoSQL system? Thirty years ago there
was a debate between people advocating SQL and people advocating
writing operations directly. SQL won because it could *compile* down
to those same operations. Betting against the compiler isn't very
smart, and high level languages (like SQL) provide better code,
independence, etc. More subtly, stored procedures are good: they let
you move the code to the data instead of the other way around, which
is faster.

If you actually need consistency, using a system that doesn't provide
ACID means you need to write it yourself. "That is a fate worse than
death." And if you don't need ACID today, can you guarantee you won't
need it tomorrow? You need ACID if any part of your problem consists
of saying "Do both A and B, or neither." Examples: funds transfers,
integrity constraints, or multi-record state.

"Eventual consistency" means "creates garbage": non-commutative
updates, integrity constraints. "What happens if someone buys the last
inventory item and the primary fails before it replicates? When that
system comes back up, you could end up with -1 in inventory, which is
usually an illegal state." Eventual consistency only works when you
can apply updates in any order and wind up with the same result.

NoSQL is fine for non-transactional systems with single record
updates. It is not appropriate for TP.

So how do you build a SQL-based, ACID-compliant system that performs
better than the legacy systems? You need a system that scales to large
clusters (one node solutions are no longer interesting), that has
automatic sharding, and that focuses on OLTP. By focusing on OLTP
problems you have, you can specialize and gain more speed. A
specialized hammer will be more effective than a generic system
attempting to be both a hammer and a screwdriver.

Storing data in main memory is great, and most main memory databases
can spill cold data to disk without significant overhead.

Eliminating the Write Ahead log ("Mohan kool-aid"): modern TP requires
high availability, which implies replication and fail over/fail back.
So there's no need to recover from the Aries-style write-ahead log.
Yabut: what if the power goes out? The WAL is the "slow" option, so
you can do periodic check-pointing, with a command log. That log might
only use a stored procedure identifier + parameters, with group
commit, meaning you log less than a traditional WAL. Recovery time is
worse, but total cluster failures are rare [hopefully].

If you eliminate multithreading, you can go even faster, because
there's no shared data structures to latch access on. For multicore
processors, your system can divide memory into n buckets, one per
core, and pretend you're on separate CPUs.

Finally, eliminate row level locking.

VoltDB:

* Subset of SQL (getting larger)
* 70X faster than legacy DBMS on TPC-C
* 5-7X faster than Cassandra using VoltDB K-V layer
* Scales to 384 cores (biggest iron they could find)


Beware of vendors who:

* Use Multi-threaded
* Implements WAL
* Uses ODBC/JDBC for high volume
