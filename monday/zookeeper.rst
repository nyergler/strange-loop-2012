Building an Impenetrable ZooKeeper
==================================

:Authors: Katheleen Ting
:Time: 2:00 pm - 2:50 pm
:Session: https://thestrangeloop.com/sessions/building-an-impenetrable-zookeeper
:Slides: https://github.com/strangeloop/strangeloop2012/blob/master/slides/Ting-BuildingAnImpenetrableZooKeeper.pdf?raw=true

ZooKeeper is the unsung hero, and a lot of time people don't know that
it's there until it's down. Because ZooKeeper is so important, it's
important to make it durable.

ZooKeeper is fairly stable, so more often the things that bring ZK
down are misconfigurations, not bugs.

ZooKeeper is a coordinator for distributed applications. It is
designed to remove the need for custom coordination code/solutions. ZK
is used by HBase, HDFS, Solr, Kafka, etc.

Misconfigurations are any diagnostic ticket that require a ZK/config
file change. These comprise 44% of tickets at Cloudera [eep!].
Typically ZK is straight forward to set up and operate, and issues
tend to be client rather than ZK issues.

A 3 ZK Ensemble consists of three ZK machines: one leader, two
followers. All three store a copy of the same data. This full
replication ensures durability.

Leader is elected at startup, changes are coordinated through the
leader, and clients talk to followers. Changes are accepted when a
majority of ZKs agree.

Common Misconfigurations:

#. Too Many Connections

   * ZK has a limited number of connections; defaults to 60 [per IP?]
   * HBase clients have leaked connections in the past, so they have to
     be closed manually

#. Connection Closes Prematurely

   Need to increase wait time for recovery. [Didn't understand this
   completely.]

#. Pig Hangs Connecting to HBase

   * Caused by Pig not knowing the location of the ZK quorum.
   * This can be resolved with Pig10

#. Client Session Time Out

   * ZK defaults session timeout to 40s, while HBase needs a 180s
     timeout for garbage collection.
   * This will cause them to agree on the shorter session timeout
   * HBase will begin to timeout under IO load, because it needs more
     time
   * This may also be caused by co-locating ZK with something IO
     intensive like a DataNode or RegionServer
   * ZK has relatively low IO requirements, but durability requires
     that changes fsync before reporting as accepted.

#. Clients Lose Connections

   * The ZK transaction log is optimized for mechanical spindles and
     sequential IO
   * SSD provides little benefit to ZK, and suffers from latency spikes

#. Unable to Load Database: Unable to Load Quorum Server

   * If there is disk corruption, ZK will refuse to load
   * If you have two other running ZK instances, you can safely wipe
     the database and it will replicate from the other two when it
     comes back up

#. Unable to Load Database - Unreasonable Length Exception

   * ZK allows a client to set data larger than the server can read
     from disk
   * ZK includes the metadata when calculating the size
   * Increase the max size to work around
   * This is a bug in ZK which will be fixed in a future release

#. Failure to Follow Leader

   * If your ZK nodes comes up and is not the leader and can not
     contact the leader, it will not be able to restart

Because ZK operates by majority, recommend having an odd number of
servers in an ensemble: if you have 2 servers in an ensemble, and one
goes down, you're down (1 is not a majority of 2). Recommend:

* 1 if you only want coordination
* 3 if you want reliability for production environments
* 5 if you want to be able to take one down for maintenance

But more isn't always better: more servers means you need to wait for
more votes in elections. You can use Observers to provide more
followers that do not participate in elections.

You can verify the configuration using zk-smoketest.

Best Practices

* Separate spindles for dataDir and dataLogDir -- improves latency and
  avoids competition
* Allocate 3 or 5 servers
* Run zkCleanup.sh via cron
