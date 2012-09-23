Roy
===

:Authors: Brian McKenna
:Time: 1:20 pm - 2:00 pm
:Session: https://thestrangeloop.com/sessions/roy
:Link: http://roy.brianmckenna.org/

Roy is an altJS language (http://altjs.org). While he was compiling
the list of languages, he played with lots of them, and none of them
satisfied his

Writing correct JavaScript is hard, partially because everything is
mutable. So Roy is CoffeeScript meets OCaml meets Haskell.

console.log "Hello World"

console.log ("Hello World");

Roy is statically typed::

  console.log ("40" + 2)

Won't work in Roy -- incompatible types.

::

  let f x : Number = x

Defines a function f which takes a Number ``x`` and returns a Number
(inferred in this case).

Roy preserves comments in your source.

You can also do "static duck typing": extensible records. You can
specify which properties an object needs to have, then anything with
those properties can be used.

This lets Roy provide pattern matching.

Roy provides a module system. When compiling to a browser module,
those are assigned to the global scope. But you can also compile to
CommmonJS, which will use the require system, as well as an async
module def.

Future of Roy
-------------

* Functional Lenses
* Deferred type errors
