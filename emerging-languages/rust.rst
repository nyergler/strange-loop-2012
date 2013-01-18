Rust
====

:Authors: David Herman
:Time: 2:40 pm - 3:20 pm
:Session: https://thestrangeloop.com/sessions/rust
:Link: http://www.rust-lang.org/
:Slides: https://github.com/strangeloop/strangeloop2012/blob/master/slides/elc/Herman-Rust.pdf?raw=true

A Haiku to describe Rust:

a systems language

pursuing the trifecta

safe, concurrent, fast

Mozilla Research is interested in the platform side of the web: Boot
To Gecko, for example. And at a lower level, thinking about how they'd
design things for the future. Looking at a set of tabs in a browser,
you think about them in terms of security and sandboxing, but Mozilla
Research thinks about them in term of threading, performance, etc:
like the browser is an operating system that provides a bunch of stuff
for "apps".

So why Rust? Mozilla's codebase is *huge*, and lots of C++, which is
not designed for security, etc. Started working on Rust in 2009, self
hosting on LLVM in 2011. Rust is the love child of C++, Erlang, and
Caml, and Haskell.

Fast
----

Sometimes abstractions remove duplicated code, but introduce
performance issues. Rust strives to provide zero cost abstractions.
For example, lambdas are aggressively inlined, and you can annotate
code to push the compiler towards inlining. Rust also provides C++
like structured data: direct access instead of indirection. [Shows
example of a ``Point(x,y)`` struct. You *can* deal with the
indirection if you needed. Rust is currently calling these "borrowed
pointers" to reinforce the idea that they're on the stack, and not
necessarily long lived. Rust also provides syntax for allocating on
the heap, when needed. These can be passed as borrowed pointers, if
needed.

Four ways to use a structure:

* Directly
* Borrowed Pointer (*)
* Heap Pointer (&)
* Unique (Owned) Pointers (~)

You can "move" ownership of pointers; doing so effectively eliminates
the local variable so you no longer have access.

Concurrent
----------

Actor-like language, so you allocate tasks. When you create a stack,
you have a small stack allocated, which will dynamically grow. Task
creation is pretty cheap. Tasks can not have pointers to data in other
tasks, so they can be garbage collected independently. [Sounds like
they're roughly the same as processes.] Tasks can allocate unique
pointers, which are created on the shared heap. When a Task needs to
communicate with another Task, it simply ``moves`` the pointer to the
other Task. This pointer pass is very cheap. And because it's a unique
pointer, you're guaranteed that no other Task points to it.

Safe
----

Rust has generics (parametric polymorphism). "Classes" are flat
structs, and you can attach methods after the fact.

You can declare ``traits`` which provide a way to do Type Classes
[feels like interfaces to me].

ARCs ("Automic Reference Counting") structs require that the data
within it must be deeply immutable. These can be freely shared between
Tasks.

Even though Rust is designed for safety, there's no such thing as a
completely safe language: every language has a way to do something
"unsafe". Rust has that, too, but they're branded with a huge hazmat
sign, and if you touch them, your code is branded unsafe.

http://smallcultfollowing.com/babysteps

http://pcwalton.github.com/
