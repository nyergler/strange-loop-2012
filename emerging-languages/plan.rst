Plan: A New Dialect of Lisp
===========================

:Authors: David Kendal
:Time: 11:00 am - 11:30 am
:Session: https://thestrangeloop.com/sessions/plan-a-new-dialect-of-lisp

"A look at the future direction of programming languages."

Starts by talking about "every programming language ever". We usually
think about languages in terms of "System" (fast, low-level, static,
compiled, "fast") vs "Scripting" (high-level, dynamic, interpreted
"slow"). But in the last few years it's become more common to write
"real" apps in "scripting" languages like Ruby, etc. Maybe there's a
third side: Embedded Languages. Mid-level, static/dynamic, "latched
fence" models, and often quite fast. Plan is an attempt at writing a
fast, mid-level Lisp.

So how do languages become successful? A lot of people think that
success is correlated to having a large number of libraries. But there
are a small number of built-in libraries that are actually really
important for composing larger systems: HTTP, JSON, etc in modern
systems. And a successful language will have a way to distribute
modules (i.e., CPAN).

Traditionally Lisp didn't support a lot of polymorphism for types, and
you still see this in Scheme. Plan addresses this by doing pattern
matching for types, and tagging objects with type metadata. Plan would
like to provide a comfortable path for people working today in Python
and Ruby into Lisp-i-ness.

Some familiar syntax can be transparently converted to Lisp syntax::

  a[b]

    => (get-prop a b)

  (set a[b] c)

    => (set (get-prop a b) c)
