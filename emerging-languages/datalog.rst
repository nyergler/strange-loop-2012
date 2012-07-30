The Reemergence of Datalog
==========================

:Authors: Michael Fogus
:Time: 12:40 pm - 1:20 pm
:Session: https://thestrangeloop.com/sessions/the-reemergence-of-datalog
:Slides: https://github.com/strangeloop/strangeloop2012/blob/master/slides/Fogus-Datalog.pdf?raw=true

Also known as "Return of the Living Datalog". Michael likes turtles.

A common way we look at data is as a "rectangle" (table).
Rectangulation falls down a bit when describing relationships, sparse
data, multi-valued, and leads to Place-Oriented Programming (PLOP).

Dealing with data in Java often involves lots of code that obscures
what the data actually is: you write a lot of code to accomplish what
you want, and mistake the menu for the meal. So Java programmers try
to make the data more familiar/comfortable, by mapping it into what
they understand: classes. "ORMG!" This reinforces a false dychotomy
between code and data.

So how do we unify code and data? Unification. With unification you
"punch holes" in your data and provide places for variables. Then you
can try to fit data in, like a key in a lock. Unification diverges
from pattern matching because it can leave variables there: you don't
have to match everything, or the unification can introduce new
variables.

Unification closes the gap between data and code, but doesn't quite
get us to the point where you can *use* the data in what we'd normally
consider a program. Prolog is one of the ways we try to close that gap
completely. Prolog programs assert facts, and use rules to reason: X
spawned Y, and if A spawned B, B is a descendant of A. This is great:
our data can now sort of be treated as code. But it has a few
problems: clause-order dependence, non-termination, and imperative
infection.

Datalog is a query language (not general purpose, not Turing
complete), very explicit with its bindings, and relatively simple.
Datalog began its life in 1977, and work was done until 1995 when it
was declared "not relevant". In 2002, though, it began to gain use as
a way to describe security and topologies.

Datalog is a logic programming language with recursive queries and
recursive joins. Datalog works off a simplified Entity Attribute Value
(EAV) model. This EAV model means Datalog is suitable for querying
sparse datasets.

Datomic is an implementation of Datalog which removes the need for a
database, and introduces the notion of "time travel". Instead of
always specifying a database, Datomic allows you to pass in a set of
raw data. This makes it useful to test your queries. Keeping track of
time in a relational database can be tricky. Datomic allows you to
specify a fourth field in the tuple as a time field (actually
transaction). This allows Datomic to perform total ordering of
transactions, and allows you to bound queries by time.

Daedalus is also a Datalog implementation with a notice of time,
although it's different from Datomic. Because it's designed to support
distributed processing, Daedulus is based on a tick model (with some
accommodation for unreliable network connections).

A third implementation, Cascalog, is written in Clojure, and provides
map/reduce processing. This also means you have order independence.

Bacwn is another Datalog (also Clojure based?) which provides
negation. Negation utilizes a NOT predicate, which lets you take the
same query and return the logical "inverse". [Shows example using
MST3K characters, querying first for all characters on the SoL, then
those not on the SoL.]

So what about query time? Query plans provide one way to do things,
but we don't get any guarantee that that's what the engine will do.
You can hint your query, but that's a black art. Prolog requires you
order for termination, but Datalog requires that you order for speed
(many naive Datalog queries will run slowly). Pluggable optimizers may
be the way forward: plug in an optimizer than knows your own data
without impacting other Datalog optimization techniques.
