Functional Design Patterns
==========================

:Authors: Stuart Sierra
:Time: 11:00 am - 11:50 am
:Session: https://thestrangeloop.com/sessions/functional-design-patterns
:Slides: https://github.com/strangeloop/strangeloop2012/raw/master/slides/Sierra-FunctionalDesignPatterns.html

Had the idea for the talk about six months ago at Clojure West. When
he went to write it, he realized that "design pattern" is sort of a
loaded term. People associate the term with the "Gang of Four", which
"sounds like an ominous cult, or worse, a Senate committee." GoF is
the product of its time, and described a lot of great starting points,
which people took and corupted. Some people say that design patterns
are an anti-pattern: that if your language needs them, your language
has a problem.

In 1996, Norvig gave a talk where he talked about how most of the
patterns in GoF are invisible or grossly simplified in dynamic
languages. But then goes on to talk about how at one point a
sub-routine call was also considered a "design pattern".

Going to focus on patterns that show up slightly differently in
functional languages. And he's not going to talk about Monads, even
though some of the patterns he'll describe are monadic.

Monads are useful for writing programs, but he doesn't find them very
useful for *explaining* them.

Architectural Patterns: describing an entire system

Design Patterns: describing a specific task/operation

Idioms: low-level patterns specific to a programming language

State Patterns
--------------

State/Event
~~~~~~~~~~~

* Derive state from previous state + input
* Need to recover past states (and perhaps inputs)
* Need to visualize intermediate states

This is the pattern of a single function that takes a state and an
event. Modeling your state this way is powerful -- it allows you to do
things like take the starting point and all inputs and reduce to the
end state. Makes it a great pattern for testing systems. Allows you to
make assertions about the state of the system over time. You also have
a lot of flexibility about how you store the state: at one end of the
spectrum, you only store inputs/events, not state, since it's derived.
One of the downsides is that every input/event in your system has to
be a data structure. That's sort of the point, but it can add
complexity.

Consequences
~~~~~~~~~~~~

* An input to the system can cause multiple events/side effects
* Generated events can trigger state change

One function takes state + input, and returns a sequence of events.

Another applies that sequence of events to state using reduce.

You need to decide if you're going to allow recursive consequences.

A problem with this is that you can't just compose the consequences to
get to the current state.

Data Building Patterns
----------------------

Accumulator Pattern
~~~~~~~~~~~~~~~~~~~

* Large collection of inputs -- maybe larger than memory
* Small or scalar result

One of the essences of functional programming: lazy sequences -- map,
mapcat, filter, etc -- and ``reduce``.

This has a built-in assumption of ordered, linear processing, that
you're going to deal with things one at a time.

Reduce/Combine
~~~~~~~~~~~~~~

* Input is tree-like
* Divide and conquer approach
* Associative combination of intermediate results
  (a + b) + c = a + (b + c)

Utilizes a reducer and a combiner function. The combiner provides a
way to "roll up" one level to a level "up". Doesn't assume linear
processing (hence the associative requirement). In some simple cases
(addition, for example), the reducer and combiner may be the same
function.

Recursive Expansion
~~~~~~~~~~~~~~~~~~~

* Build a result from primitives
* Abstractions are built in layers
* Recurse until there's no more work
* Examples: macro expansion, Datomic transaction fns

A function takes an expander and some input, and calls expander with
input (and after the first call, the result of the previous call),
until the return value equals the input value.

Flow Control Patterns
---------------------

Pipeline Pattern
~~~~~~~~~~~~~~~~

* Some process with many discreet steps
* one execution path -- no branching
* Each step has a similar "shape" -- a map or record in Clojure

Because each step needs to take and return the same "shape" of data,
the code can wind up being a little longer. But the result is very
clear: you can easily see the steps that are being taken. And because
you have to work with the same shape of data, the resulting pipeline
is composable into other, larger pipelines

Wrapper Pattern
~~~~~~~~~~~~~~~

* Similar to the Pipeline
* Possible branch at each step

Instead of composing a list of functions (steps), you use higher order
functions that could do something before or after an individual step.

Because each step can do things before and after, it can become
difficult to reason about where something is happening.

Token Pattern
~~~~~~~~~~~~~

* An Operation may not have an identity
* But you may need to cancel it

So you wrap the operation with something that returns a "token" --
something that can cease the operation and get you back to your
original state. The scheduled thread pool in Java works this way.

Observer Pattern
~~~~~~~~~~~~~~~~

* Register an observer with a stateful function

The observer could take the old and new state, along with either the
delta, the triggering event, or the container.

Strategy Pattern
~~~~~~~~~~~~~~~~

* Many processes with similar structures
* Extension points for future variations
* This is a GoF pattern which starts to disappear in Clojure

Clojure protocols are an implementation of this. Another way to do
this is by passing around a map of the functions. This *feels*
functional, but it has some performance overhead: every invocation
requires a map lookup.
