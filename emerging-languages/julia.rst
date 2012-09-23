Julia: A Fast Dynamic Language for Technical Computing
======================================================

:Authors: Stefan Karpinski
:Time: 2:00 pm - 2:40 pm
:Session: https://thestrangeloop.com/sessions/julia-a-fast-dynamic-language-for-technical-computing
:Link: http://julialang.org/

Julia is a fast, dynamic language for technical computing. "Technical
Computing" is sort of a made up term, but it includes languages like
Matlab, Maple, Mathematica, R, SciPy, etc. There are at least forty
technical computing languages. Julia had three feature goals: dynamic
language, sophisticated parametric type system, and multiple dispatch.
Existing languages have some of these, but the combination is somewhat
unique in Julia.

Julia can be Matlab-like: code is defined in functions, [shows
example]. But you can also write lower-level, non-vectorized code
[shows example of their quicksort micro-benchmark]. Julia supports
distributed computation, and macros for constructs _like_ distributing
computation.

Shows the difference between how Python and C store arrays. C requires
that arrays have a single type, so you can store (say) the list of
floating point things in contiguous memory. Python lets you do
anything in the list, so it stores a list of pointers to the elements.
Julia wanted to be able to store the information contiguously in
memory, and still have flexibility. This means you can't change a type
once it's declared: you can't make it bigger, add fields, etc. [Did I
mishear that this means there's no/limited sub-classing?]

Discussion of how Julia handles multiple dispatch. Uses explicit
promotion instead of overloading.

[ Live coding demo of hypothetical Modular Int type. ]

Julia performs very well compared to Python, Matlab, Octave, R. Julia
runs on LLVM, so fast to start with, and they're working on performance.

Julia performs no static type checking.
