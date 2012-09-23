Elixir: Modern Programming for the Erlang VM
============================================

:Authors: Jose Valim
:Time: 4:30 pm - 5:10 pm
:Session: https://thestrangeloop.com/sessions/elixir-modern-programming-for-the-erlang-vm
:Link: http://elixir-lang.org/

Why Elixir? First, the Erlang virtual machine is great, so Elixir attempts to
expose the great parts of that VM in a different way, while addressing
some of the shortcomings of the host language. The Erlang VM was built
for concurrency and for hot deployment of updates. Second, multi-core
is here to stay. Erlang was built for concurrency, and a lot of the
features that make it great at that also make it great at supporting
multi-core hardware.

The goals of Elixir are:

* Productivity

  Elixir attempts to increase productivity by eliminating boilerplate
  code. Everything in Elixir is an expression, which makes the model
  more flexible. Elixir also supports macros. The combination of the
  two features means that domain specific languages (DSLs) are easy to
  develop in Elixir. [Shows example of a test case DSL.] Macros also
  support pattern matching.

* Extensibility

  Elixir's goal of extensibility is a direct critique of Erlang. This
  is accomplished through the use of Protocols.

* Compatibility

  Being compatible with existing Erlang tooling is an explicit goal of
  Elixir. There is no conversion cost for calling Erlang from Elixir
  and vice-versa.

  Elixir works out of the box with existing code like OTP.
