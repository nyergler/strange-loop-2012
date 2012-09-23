Symbiotic Languages: Transpiling into JavaScript
================================================

:Authors: Jeremy Ashkenas
:Time: 9:30 am - 10:00 am
:Session: https://thestrangeloop.com/sessions/symbiotic-languages-transpiling-into-javascript


"I like to think of this as the place where the back wood medicine folk
of programming languages get to come out of the woodwork and talk
about what they're doing."

Only a few people here since the first emerging languages camp, which
was in a cramped room in the first floor of OSCON. What made it great
was discussion, people asking questions. So do that.

Here today to talk about Symbiotic Language: languages that "compile"
to the source code of an existing language. These are interesting
because writing VMs is really, really hard, and we've developed some
strong VMs that might be 15-20 years old, but they're still useful.
HotSpot/Java, V8/Dart. Using an existing VM means you get to pick some
of the properties of the host language you think are useful, and
cherry pick those, and leave behind the ones you don't want.

There's a spectrum of options between compiling and *trans*piling.
Transpiling means just translating some language to the source (or
occassionaly the byte code) of the target, host language. Retains a
lot of the semantics of the host language, since you're working
primarily with keywords and tokens. Once you start to change the
semantics (i.e., by implementing "special functions" in your language
that the new language calls into to implement). CoffeeScript tries not
to add these "features", because they want to stay close to the
JavaScript semantics.

Charlie Nutter, who works on JRuby, has explored this: he implemented
99.5% of the Ruby semantics on Java. He's also tried the other
approach: implementing Java semantics with Ruby code.

CoffeeScript maintains a wiki of different languages that can be
compiled into JavaScript. It's a long list!

Jeremy's experience is primarily around CoffeeScript: "It's *Just*
JavaScript". JavaScript has proven remarkably versatile and robust for
something designed in about ten days. CoffeeScript transpiles to that,
and has come a long way since it was first developed during the first
Emerging Langauges Camp [shows graph of StackOverflow vs GitHub from
RedMonk; CoffeeScript has grown to something like #11 in two years].

So the choice is what you want to preserve vs deviate. One thing they
can't do is negative array indices: you'd need to inject some function
into the transpiled source code to support it, since you don't have
nearly enough information at compile time.

One thing they *can* do is "everything is an expression". Lots of
existing Javascript is expressions, but there are also control flow
objects that aren't in plain JavaScript. CoffeeScript enables this by
function wrapping: shows examples of using a complex if, a try/catch,
and a loop as an expression.

The other semantic change CoffeeScript makes is the addition of
classes, which CoffeeScript transpiles into the appropriate prototype
declaration. You can also do interesting things like executable class
bodies, which allows you to do interesting things like change the body
of a class based on some value. [shows example of a Pirate class that
speaks in English if century > 1700, otherwise Spanish]

The politics of programming languages are such that even if you build
something interesting, there are significant barriers to adoption: "I
have to use the JVM", "It doesn't work on the web", etc. By
transpiling you give people a shim to start playing with your new
language. CoffeeScript is a really interesting case study of this:
it's come very far in two years without corporate support or backers.

It used to be that you learned a new language to program on a new
platform. With symbiotic languages, we're building the same sort of
systems we were already building, but hopefully doing so in a more
expressive, clean, powerful way. This means our code has two
audiences: other programmers, and other programmers in the target
language [I think I got this right] -- and by extension, the target
platform compiler.

});
