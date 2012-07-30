A Type Driven Approach to Functional Design
===========================================

:Authors: Michael Feathers
:Time: 1:00 pm - 1:20 pm
:Session: https://thestrangeloop.com/sessions/a-type-driven-approach-to-functional-design

Been thinking about how we design functional programs. It's gained
ascendancy in the past 5-10 years, but you don't hear people talk
about how you *design* those programs. This is in contrast to
ascendancy of OOP, when everyone had an opinion they were happy to
share.

Had this thought that the Haskell type signature was useful for
describing how to assemble programs. Wrote some Ruby code, and even in
Ruby he was adding comments to describe expectations that *looked* a
lot like Haskell type signatures. For example::

  map :: (a -> b) -> [a] -> [b]

Describes the function ``map`` that takes a function as a parameter
that takes a and reduces to b; it can also take a list of "a" and
return a list of "b". There's no clear demarcation between input and
return, the return value just happens to be the last thing returned.

::

  region 7 9 "expersexchange"

  region :: Int -> Int -> String -> String

So ``region`` is a function that takes two indices and a string, and
returns another string.

Thinking about a line break algorithm in these terms. Describing the
steps in terms of types helped him understand the "shape" of the data.
