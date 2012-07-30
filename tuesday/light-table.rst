=================
Behind the Mirror
=================

:Authors: Chris Granger
:Time: 10:00 am - 10:50 am
:Session: https://thestrangeloop.com/sessions/behind-the-mirror-the-birth-of-light-table
:Link:

In 1974 people at the MIT AI Lab were writing code using TECO -- the
Text Editor and COrrector (actually "Tape" Editor, because that's the
medium they were using). What made TECO interesting was that it wasn't
an editor like we think about it -- it was a *language* for text
manipulation. In the original paper for TECO they coined an interested
acronym -- YAFIYGI: "You ask for it, you get it."

One of the people at MIT thought like we might today -- that this was
cumbersome. He visited the Stanford AI lab, and saw they had a
different way of editing text: WYSIWYG. Returning to MIT, this
individual -- Stallman -- begin writing macros on top of TECO to
create a more functional WYSIWYG editor. That became Emacs, and led to
a huge increase in usability.

Thirty five years later, he was hired as the program manager for
Visual Studio, eventually owning C# and VB. He was asked to think
about the future of the IDE, but the underlying question -- how do
people use it now -- didn't have a satisfactory answer. He found no
one had done an end to end analysis of Visual Studio. They'd studied
individual new features, but not the product as a whole. Granger did a
usability study of Visual Studio. One interesting thing he found is
that the people who swear they don't touch the mouse actually *did*
use it. They used it when they were reading the code, though, not
necessarily while writing it.

Granger was expecting to find that Visual Studio was too complicated,
that it's too "noisy" (distracting). There was some evidence that this
was true, but no vocalized it: no one mentioned out loud that their
attention was divided or diverted. His conclusion is that they didn't
vocalize it because they were too busy trying to do something else:
trying to keep the state of the program in their head. The primary
things they used were the editor, the explorer, and the debugger. This
felt really similar to the way things worked forty years before.

Granger set out to try and re-imagine the way tools work. But the
first work wasn't Light Table. He started learning Clojure, a lisp
that runs on the JVM. Learning/using Clojure taught him that the
important thing wasn't keeping the entire program in their head, it
was abstracting away enough of the program to keep one part in their
head. "Great programmers are able to create and traverse
abstractions." Programmers deal with abstraction -- there are
frameworks for everything, and we as programmers create and consume
abstraction. Programmers learn about abstractions by "poking" at them.

Wrote a prototype of Light Table in six days while he was on vacation,
without a network connection. People responded well, and asked to put
it on Kickstarter [really?!], and Granger expected it to fail. Instead
it raised over $300,000. His conclusion: people agree we're in the
dark, and we're disconnected from the systems we're building.

[ Demonstrates using Light Table to build a tool for showing git
status and modeling abstractions from a game he's been writing with
his brother. ]
