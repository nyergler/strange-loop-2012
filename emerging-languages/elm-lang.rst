
Elm: Making the Web Functional
==============================

:Authors: Evan Czaplicki
:Time: 10:30 am - 11:00 am
:Session: https://thestrangeloop.com/sessions/elm-making-the-web-functional
:Link: http://elm-lang.org/


The focus is making the web functional, but the real question is "why
elm?". It's inspired by the Kubler-Ross model: Acceptance, Denial,
Anger, Bargaining, Depression.

Wants to:

*  make GUI programming more
pleasant: reduce the time/headache from idea to reality, make people
ask "How was it not this way before?"

* Also wants to make programming
more accessible: no installation required, interactive compiler
online. Quick visual feedback. Examples! Easy path from novice to Expert.

These goals are accomplished through:

* functional GUIs: enforces safe
programming practice, plays nice with concurrency, beauty/elegance.

* Accessibility: target the web, be open source, great
  resources/examples


"But aren't GUIs imperative?" is the objection. Perhaps, but there's a
lot to learn from functional programming, and the fact that GUIs have
been imperative is an artifact of poor tools. Elms is an effort to get
the tools there.

A GUI is made up of computations, graphics, and reactions. The
question is how to do each of these in a functional way. We know how
functional computation works: it's pretty well understood. Graphics
have historically been very imperative, but we've been moving to
higher level abstractions, from pixels in a matrix to triangles, to
OpenGL, etc. The idea of more abstraction is to reduce the number of
steps between "I want a pentagon" and "I have a pentagon".

Elm works with ``Elements``: rectangular "things" we put on the
screen. Some basic functions that return Elements include plainText,
Image, fittedImage. Elm also supports Markdown for text formatting.

Elm attempts to make things that are conceptually simple simple to
program. So things like alignment ("put this in the middle") or flow
("put these one after another") are simple to express (unlike, say,
HTML).

[shows demonstrations, including Collage, which lets you composite
things easily.]

So graphics can be done in a high-level, compositional, functional
manner. But how do you handle reactions? If a value is immutable (in a
functional language), how do you deal with user input? Elm introduces
the idea of time-varying values: a stream of input like the mouse
position. By introducing this idea, you also get some
signaling/auto-update: things that depend on the mouse position will
automatically update when it does. Elm is functional, so you can
change the way things react/interact without changing the way the
graphics are drawn.

Elm can be used for writing games: imperative game programming is
pretty flexible (pixel flipping, etc) -- too flexible in the opinion
of the author. Elm requires you to use good structure. [Demonstrates
Pong using Elm] Every Elm game must have three parts: a model, the
state, and the view. You might think of this as the functional
equivalent of MVC.
