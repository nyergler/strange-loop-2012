The High Order Rubyist
======================

:Authors: Robert Pitts
:Time: 1:30 pm - 1:50 pm
:Session: https://thestrangeloop.com/sessions/the-higher-order-rubyist

Functional programming in Ruby

So why bother? Ruby lets you write succinct code and easily create
DSLs. Unfortunately as you write larger systems, maintenance becomes
an issue. His theory is that by approaching problems more directly and
declaratively will help with this issue.

Ruby includes some batteries that get you started on this approach.
The Enumerable module includes common higher order functions. It
*also* includes some helpful destructing syntax.

Ruby also has some first-ish class functions that look a lot like
lambdas: blocks, procs, lambdas, [something else].

Ruby 1.9 added ``Proc.curry`` to support currying and partial
application.

Continuations previously had a bad reputation in Ruby -- poor
performance and memory usage. ``callcc`` improves that performance.
Note that continuations may be removed from a future version of Ruby,
still under discussion

Tail call optimizations are available in Ruby 1.9. Not enabled by
default, so you can either recompile or pass Ruby configuration.

In addition to these built-ins, there are other attempts to add
functional properties to Ruby.

* Hamster

  Similar semantics to core Ruby, except for some cases to address
  [im]mutability.

* Stunted

  Allows you to define bags of functions in a module, and then mix
  those into other classes. Nice for composability.

* Ruby#Facets

  Well tested and been around a while.

* Celluloid
