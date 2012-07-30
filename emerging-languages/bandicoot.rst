Bandicoot: Code Reuse for the Relational Model
==============================================

:Authors: Ostap Cherkashin
:Time: 10:00 am - 10:30 am
:Session: https://thestrangeloop.com/sessions/bandicoot-code-reuse-for-the-relational-model
:Link: http://bandilab.org

Ostap has been working on Bandicoot for about four years, mostly part
time, but recently their time has been increasing. Bandicoot is open
source, and hosted on github. Bandicoot began because Ostap was
writing code in many languages including Java and SQL. He was
fascinated because when he wrote something in SQL, you could write
something once and it would work with no records, one record,
thousands, or millions of records. And there's been lots of research
on relational systems: fast joins, etc. At the same time there's been
a lot of research on concurrency and control. But we're using a 40
year old language (SQL) with poor re-use.

Bandicoot is a set based programming system that aims to improve the
interface to relational data. Bandicoot is a new language and new
runtime for doing this.

Introduces a test case he's going to use: two CSV files, one of books,
one of discounts. Example: want to apply discount and find all books
with a price greater than 100.0$, and then list all the genres.

Bandicoot runs over HTTP: you write a program, and the runtime exposes
the functions over HTTP.

Variables define sets of data, and committed data persists across runs
of Bandicoot. Bandicoot understands CSV, so you can post data to a
function to store data.

Bandicoot supports eight relational algebra operators: four unary,
four binary. The ``select`` operator allows you to apply a boolean
filter to some set of data. ``join`` is a binary operator that takes
two sets and joins them into a single set based on some relationship.
The ``project`` operator selects distinct subsets from a set.

By allowing you to define "functions" that apply operators to sets,
Bandicoot provides a way to re-use code and composite functions.
Everything is a relation is Bandicoot, so you can easily pass things
around for composition.

Working On:

Attribute Sets allow you to define your types (schemas) in terms of
composition, as well.

Modules for grouping functionality together.

http://github.com/bandilab

http://mingle.io (try it online)
