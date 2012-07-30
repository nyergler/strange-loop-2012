================================
 Apache Cassandra Anti-Patterns
================================

:Authors: Matthew Dennis
:Time: 11:00 am - 11:20 am
:Session: https://thestrangeloop.com/sessions/apache-cassandra-anti-patterns
:Slides: https://github.com/strangeloop/strangeloop2012/blob/master/slides/Dennis-ApacheCassandraAntiPatterns.pdf?raw=true

Don't run C* on a SAN. Cassandra was designed for commodity hardware,
so it didn't really plan for SAN/high performance hardware. It's not
only unnecessary, it actually performs worse on SANs than it does on
commodity hardware. C* uses (un)coordinated IO, so each node assumes
it has local disk and attempts to maximize the bandwidth it uses. If
you try to use a SAN, you wind up hammering your SAN.

Cassandra uses a commit log used for recovery; putting it on the same
volume as the data directory causes problems because they have
conflicting IO patterns. Commit logs are 100% sequential appends, but
the data directory is usually random reads. The commit log is very
sensitive to other processes moving the disk head. This problem only
shows up under load, so it's sometimes difficult to find when testing.

Oversize JVM heaps are an issue -- 4-8G is good, 10-12 is fine
("correct" or "not bad"), 16GB is the max. Greater than 16GB is a
problem, as is setting the JVM heap to the same size as the RAM on the
box. This is due to increasing "GC suckage".

Scheduled repairs should be run with "-pr". This prevents it from
communicating work to other nodes, therefore reducing the work load
from duplicated work.

C* requires a lot of file handles, so the common default of 1024 is
absolutely not sufficient. This *does not* show up in testing, even
when testing with large datasets. It shows up with load spikes, and
fails in unpredictable ways. 32K-128K is common.

Putting a load balancer in front of C* is completely unnecessary and
only adds another point of failure. The clients will usually balance
between the available nodes on their own without this.

Sometimes people try to restrict clients to a single node. This
actually takes work, and causes problems. Don't do it.

Having an unbalanced ring used to be the number one problem
encountered. An unbalanced ring leads to hotspots on the node with a
larger range. OPSC automates the resolution of this with two clicks,
even across multiple data centers. Related to this, always specify
your ``initial_token`` instead of letting C* pick for you. The initial
token specifies where in the range of 0 to 2^127 the node sits.

The Row Cache is a Row Cache, not a Query Cache, Slice Cache, or any
kind of Cache. Asking for *less* than the entire row requires
deserialization of the cached row to pick out the pieces you want,
working against the Row Cache. If you ask for the entire Row, it will
use the cached version that's stored outside the JVM (which means you
need to take it into account when sizing memory for the machine). If
you turn on the Row Cache, ask for the entire row. Related, large
(2GB) rows are still a problem for the cache.

If you think you need the Byte Ordering Partitioner (formerly Order
Preserving Partitioner), you probably don't. [He didn't say why, just
that it's a big problem.]

Batches are set in a single message and must fit in memory on both the
client and server. This makes unbounded batches a real problem. The
best batch size is an empirical exercise based on your specific load,
hardware, data, etc.

Rotational disks require seek time -- and Cassandra was designed for
them. 5ms is a fast seek time, but remember that that's hard overhead
for your queries. SSDs solve this, but remember that this is overhead
on top of the software when you use rotational disks. Note that you
can totally run C* on consumer SSDs, doesn't need "Enterprise" SSD.

C* usually deals with Big Data, so a 32 bit JVM usually doesn't work.

If you're running C* on AWS, EBS Volumes are problematic. They have
nice features, but they're unpredictable. A better approach is
striping ephemeral drives and spinning up new nodes when one fails.
It's not clear whether provisioned IOPS EBS are a good fit.

At this point your should be running a Sun^WOracle JVM -- r22 or
later. Some people are successfully using OpenJDK, but it hasn't been
well tested.

Super Columns have  10-15% overhead for both reads and writes: the
entire super column needs to be held in memory. Most C* devs dislike
them.

DataStax offers a free version of their Ops Center -- no excuse not to
use it.

http://slideshare.net/mattdennis
