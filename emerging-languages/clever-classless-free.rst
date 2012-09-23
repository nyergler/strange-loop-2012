Clever, Classless, and Free?
============================

:Authors: - Håkan Råberg
:Time: 11:30 am - 12:00 pm
:Session: https://thestrangeloop.com/sessions/clever-classless-and-free

Invited to speak, and decided he wanted to talk about some experience
over the last few years. He comes from a consulting background, and
for a while had to live in the Java world. If not for Clojure, he'd
probably still be in the Java world. As such, he was a pragmatist.
Extreme programming background: TDD yourself to freedom, testing will
save you. Reached the conclusion that there are some mistakes in this
approach, and decided to try to move from the pragmatist side of
things to the "other" end. So he's taking two years off to explore
this.

Enumerable.Java
---------------

(Clever)

Took a break in 2010 from corporate job, traveling around Asia,
attempting to program while he traveled. Found himself in Kuala
Lumpur, near the end of his year of travel, and now it's time to start
programming :). Decided he wanted to solve the problem of lambdas in
Java. And he did this by working on porting the ``enumerable`` module
from Ruby to Java.

  map(xs, lambda(x, x.toUpperCase()));

But x and lambda are static things that don't really do anything:
``toUpperCase`` is the part that you actually care about.

[Leftover Lambda, Bill Hoyt]

At runtime you create a new class, move the byte codes over to the new
class, and replace the local references to the new class::

  map(xs, new Fn1<String, String>()
  {
      public String call(String x) {
          return x.toUpperCase();
      }
   });

This is basically "lambdas for Java 5": load time or AOT compilation.
Implemented via ASM Bytecode Macro, and triggered by annotated static
methods. But no Java syntax expansion. And using JRuby, he was able to
make it RubySpec 1.87 compliant.

(enumerable.org)


shen.clj
--------

(Classless)

Mark Tarver's Shen: A portable version of Qi II, consisting of a
kernel lisp (klambda) of 46 primitives. Mark provided two ports: GNU
CLISP and Steel Bank Common Lisp. There have been multiple ports:
Javascript, Clojure (2), Haskell, JVM. shenlanguage.org


Now
---

(Free)


As software increasingly structures the contemporary world, curiously,
it also withdraws, and becomes harder and harder for us to focus on as
it is embedded, hidden, off-shored or merely forgotten about.

 -- David M. Berry
