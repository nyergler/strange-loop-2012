==========================
 Computing Like the Brain
==========================

:Authors: Jeff Hawkins
:Time: 9:00 am - 9:50 am
:Session: https://thestrangeloop.com/sessions/computing-like-the-brain

Inspired by an article by Crick who wrote about the brain, saying that
we're missing a broad framework to interpret neuroscience data; it was
a data rich but theory poor field. Difficult getting a gig doing
neuroscience full-time, which is why he wound up doing Palm and then
Handspring.

Gave himself two tasks:

#. Discover operating principles of the neocortex
#. Build systems based on these principles

Start with anatomy and physiology, which are constraints on how the
theoretical principles could work, then you develop some principles,
and model them in software. Eventually that software gets written to
silicon. There's tons of papers published on A&P of brain that are
unassimilated by theory.

The neocortext is a predictive modeling system. It's responsible for
generating and processing our senses. And senses are not single sense"
they're arrays of senses: your retina is an array of a million
sensors, streaming data in at an incredible rate. The brain is born
with incredible capacity, but no knowledge. So the brain has to build
a model of the world. When you see something -- like someone speaking
on a stage -- your brain is invoking the model and making predictions
about what will happen next, and using those to detect anomalies and
deltas. And finally it generates actions, like speech. The brain is
not a computing system, it's a memory system.

Top three principles of the neocortex:

The neocortext is a hierarchy: sensory information bubbles up the
hierarchy, and then signals are pushed back down. And it's interesting
because it appears that everything in the neocortex works the same: a
single algorithm for sight, hearing, touch.

The primary memory in the neocortext is sequence memory. When you
speak, you're playing back things you've learned in time sequence. And
when you hear something you're processing a time sequence of inputs.
Even vision works this way.

The brain uses sparse distributed representations. At any given time
only a few cells are used.

Computers typically use dense representations: a few bits, using all
combinations of 1s and 0s. The individual bits don't really mean
anything -- the representation is given meaning by the programmer.
Sparse distributed representations (SDRs) have thousands of bits at
minimum, with few 1's, mostly 0's. Roughly 2% are active at any time,
but each bit has semantic meaning. That meaning is *learned*, not
assigned. When you want to represent something in the brain, the brain
picks the top best matches of "bits" for that information.

SDR has some interesting properties: you can compare two SDRs, and if
they have shared 1 bits, they have semantic similarity. Because they
are sparse structures, this is unlikely to happen by chance. You also
don't need to store *all* the bits (since they're mostly 0), you can
store the indices to the positive bits. You can also sub-sample --
it's mathematically demonstrate that it's OK to only store the top 10
bits. Even if you have a false positive, it's unlikely to happen, and
if it does, it's going to be semantically similar (so not really a
false positive). Finally, if you take the union of a set of SDRs,
you can compare any new SDR's positive bits to the set union's
positive bits and accurately establish membership. Intelligent
machines will be built on SDR.


Sequence memory has properties that act as "coincidence detectors". If
the same stimuli arrive at the same time, they have a large impact on
the cell body. If they arrive one after another, they do not. The cell
can "or" them together to determine when a coincidence occurs.

Cells become active from input from the world, and then form
connections to a sub-sample of previously active cells. That allows it
to predict its own future activity. Multiple predictions can occur at
once. Sequences of predictions are established using "layers" of cells
-- with 40 active columns and 10 cells per column, you get 10^40 ways
to represent the same input in different contexts. This allows the
brain to understand the difference between "two", "too", and "to".


To build an online learning system, you have to train on every new
input. If a pattern does not repeat, forget it. If it repeats you
reinforce it. For many years we thought of learning as the
strengthening of synapses. That happens, but we know today that
synapses *grow* (in a matter of seconds), so it's more useful to think
of synapses as forming and unforming.

These models are being applied to predictive analysis. Today we take
in data and store it in databases, and then build models and
visualizations. The challenges are data preparation (velocity is too
fast), model obsolescence, and the lack of people who can do the work.
The future of this is taking data streams and feeding them to online
models which lead to actions. The requirements of that application are
automated model creation, continuous learning, [something else].

Grok is Numenta's engine for acting on data streams. The product feeds
data streams through encoders to generate SDRs, feeds them to sequence
memory, predict anomalies, and generate actions.

Users create the data stream, and define the problem -- what to
predict, how often, and how far in advance.

[Shows examples of Grok applications]

Predictions aren't either right or wrong -- there's subtlety, and even
missed predictions (or things that happened that you didn't predict)
can help train the system.

Future of Machine Intelligence

More theory that needs to be developed --- sensory/motor integration,
attention, more hierarchy research. (People used to think there was a
motor "part" of your brain, but we now know that every part of the
brain has motor output.)

Today we're building these models in AWS, but you can imagine in the
future you could build distributed hierarchies using distributed
sensors or networks, much like the brain is hierarchical.

Currently need to do lots of tricks to make this fast in software, but
talking to hardware companies about how they might make it faster,
cheaper, and lower powered. Interesting implications for memory, but
interconnects are a little challenging -- chips aren't good at lots of
connections like these have (but sub-sampling and sparsity might help).

The applications today are around prediction/anomaly detection. The
class applications that people think of are speech/language/vision,
but not sure that's very interesting. The interesting thing to him is
building systems that can work faster than the brain.
