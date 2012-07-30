Monad examples for normal people
================================

:Authors: Dustin Getz
:Time: 10:00 am - 10:50 am
:Session: https://thestrangeloop.com/sessions/monad-examples-for-normal-people-in-python-and-clojure
:Link:

Large codebases are copmlex. Technologies like Spring, EJB, AOP, etc
all had a common goal: make the code look more like the requirements.
If you write composable functions, your boss could write the code: it
reads like the real requirements. But in real life, you have to live
with NullPointerExceptions. You can write a bind or a pipe higher
order function that wraps some of this complexity.

The big picture goal is to write code that looks like the business
logic. The difference between an API and a DSL is how well thought out
and flexible it is.


Monads are a design pattern for composing fucntions that have
incompatible types, but which are logically composable.


[This talk moved very quickly and presupposed quite a bit of
knowledge. I was unable to keep up with notes, but reading the
Wikipedia page on Monads
( http://en.wikipedia.org/wiki/Monad_(functional_programming) ) was
useful.]
