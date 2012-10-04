===============================================
Expressing Abstraction - Abstracting Expression
===============================================

:Authors: Ola Bini
:Time: 3:30 pm - 4:20 pm
:Session: https://thestrangeloop.com/sessions/expressing-abstraction-abstracting-expression
:Slides: https://github.com/strangeloop/strangeloop2012/blob/master/slides/sessions/Bini-ExpressingAbstractionAbstractingExpression.pdf?raw=true

Part of the group of people who took JRuby from a "toy" to "real
application" level. Since then he's done things from writing a YAML
parser to regex engines to re-implementing OpenSSL on Java ("that was
sort of complicated"). Since then been thinking about programming
languages.

When he started on JRuby he was working with Java during the day, with
a background in Lisp, and wanted something different. After JRuby he
began working on AIoki (sp?), a language experiment designed to
explore expressiveness.

Three questions come to mind when thinking about expressiveness:

#. Why are new languages still being created?
#. Is it worth choosing languages strategically?
#. Does language actually matter?

Expressiveness is defined as effectively conveying thought or feeling.
Focusing on efficacy is a good place to start when evaluating
expressiveness. An expressive language is a language that makes it
easy to put my thoughts down into code without a lot of steps in
between.

An alternate definition is "a language construct is expressive if it
enables you to write an API that can't be written without the
construct." where "write" implies "use", and where there's some large
restructuring needed if the construct doesn't exist.

But beware the Turing Tar Pit: where everything is possible but
nothing is easy [shows quote re: including buggy subset of Lisp].

A lot of thinking on languages cites Paul Graham's Blub Paradox, which
states you have a scale with languages placed on it from least to most
powerful. If a programmer is using a language Blub roughly in the
middle of the scale, she can't accurately evaluate the expressiveness
and power of a language higher up on the scale. She doesn't have the
context or knowledge to do so.

Aspects of Expressiveness

(these are really dimensions -- scales)

Regularity, readability, learnability (not sure if that's actually
interesting for the question of expressiveness, and maybe it's a
derivative of regularity and readability).

But the core is Essence vs. Ceremony. Everything I have to say not
related to my problem is Ceremony, and it's in my way.

Precision vs. Conciseness: if you only want to say the things you need
to say, you also need to be OK with leaving out some parts that
influence other parts of the program (precision).

For his experiments he chose expressiveness over performance, and
unsurprisingly, the language ran quite slowly. And he thinks he made a
mistake: performance is a part of expressiveness. If your language is
concise and lets you write the essence of something, but runs too
slowly to be of practical use, it's of limited value.

The theoretical side of expressiveness: "More expressive means that
the translation of a program with occurrences of one of the constructs
C to the smaller language require a global reorganization of the
entire program."

Some people say that if you have "patterns" in your language, then
your language is deficient in some aspect of expressiveness. That's in
contrast to the current thinking in the Java community, which states
that patterns are to be used to enhance understanding.

Practical Expressiveness
========================

Abstraction is slightly more well defined in programming languages,
and in most cases abstractions add to the expressive power of a
programming language. There are several types of abstractions we use
day to day. Objects are one of the most common. And abstracting
classes of objects (esp in prototype based languages) is pretty
common, too (the joke about every Scheme programmer writing their own
Class system). Macros are an abstraction over the structure of code.

One thing you don't see a lot of is abstracting the relationships
between things. There are some examples -- Actors in Erlang, dataflow
variables in Mozart [?], and Java FX -- but it's the exception rather
than the rule. Spreadsheets are actually an example of this -- cells
provide an abstraction over the relationships between values.

If your language doesn't have a Macro facility, then all of the
abstractions that add expressiveness by hiding ceremony aren't
available to you.

But not all macros are created equally. C-style macros are pretty
limited, and are little more than text replacement. Lisp macros, "AST
Macros", aren't actually AST macros. They operate on an S expression,
which is an abstraction of the AST. C++'s template system is a Turing
Complete template/macro system [yikes!].

Static typing actually is a way of expressiveness in a language, but
it's double-edged.

Generics -- and Type Classes in Haskell and Scala -- are a powerful
feature of a language that are an abstraction in and of themselves,
but they also enable additional abstractions. This makes them pretty
interesting to study.


Abstractions in general are leaky. You see this clearly in
object-relational mappers. There are two classes of ORMs: those that
try to completely hide the fact that you're operating against SQL, and
those that are closer to the metal. An example of the latter is
ActiveRecord in Rails. [My instinct is that Django's is like this,
too.] The difference between these two approaches is that in systems
like Hibernate (an example of the former), you *know* how to solve a
problem using SQL, but you can't get low enough to fix it.

Spolsky's Law: "All non-trivial abstractions, to some degree, are
leaky."

So Spolsky is probably right, but *why* are they leaky? Abstractions
are relative to what you're trying to do: they have context. They're
not absolutes. You can imagine different libraries approaching an
abstraction differently, depending on how they expect to be used.
Abstractions hide things, but only in one direction. You can think of
the leakiness as coming from the sides, issues that are orthogonal to
the one the abstraction was created against.

Linguistics
===========

Simile is sort of a type class, it's a way to add a new meaning of
something, or add abstractions.

Redundancy is something we see in natural language that we don't see
in programming languages. If you count how many times "I have a dream"
appears in that speech, it's a lot! And we do it with purpose in
linguistic language, unlike in programming languages where they
usually wind up being ceremony (see pre-Java 7
declaration/instantiation of parameterized types).

You also have a lot of different ways of saying the same thing in
natural languages. Sometimes that's true in programming languages,
sometimes it's not. Ruby and Perl let you say certain things in many
ways, while Python tends towards one "right" way. In natural language
you use different ways to say the same thing to provide additional
context, or expressiveness.

In linguistics we talk about syntax, semantics, and pragmatics. Syntax
is pretty understandable, and we know that semantics is how
identifiers relate to one another. Linguistic pragmatics is less well
known, and is how the context of something contributes to meaning, how
the context influences our choice of how to say something.


At the end of the day, natural and programming languages are about
communication. [We write for the next engineer.] We need to
communicate with team members as well as the computers that run our
code. We communicate indirectly to people paged in the middle of the
night due to a bug [:)].

One of the ways we can change the way we communicate is through
syntax. Syntax is actually more important for communicating than it is
for computers: you'll find an entire PLT community that says syntax
doesn't matter. Just as there's syntactic sugar, there's syntactic
salt (that which makes your code look bad), and syntactic sacharrrine
(which feels like overkill -- too much sugar).

Design Principles
=================

* One paradigm
* Minimal core/concepts
* Simplicity
* First Class functions
* Flexibility
* Skinnable type system

So how far away is the truly expressive language? It's not clear, and
it's not clear that expressiveness is *always* better. Maybe it's
already here, just not evenly distributed. Expressiveness and
abstractions are relative, both to the people using it and the
subjects they're being applied to. So maybe what you want is a
meta-expressive language. This is one of the reasons DSLs have become
so popular.
