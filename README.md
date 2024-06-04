# ISC Working Group 'Marshaling and Serialization in R'

Welcome to the [R Consortium ISC Work
Group](https://www.r-consortium.org/all-projects/isc-working-groups)
on Marshaling and Serialization in R started in May 2024.

---

# :calendar: **We're meeting in person on Thursday 2024-07-11 @ 15:30-17:30 UTC+02h at useR! 2024, Salzburg, Austria. The room is "Wolfgangsee"**

---

## Background and Problem

_Serialization_ is a useful tool for saving R objects to file, or
sending them to another R processes. For example, when saving an
object using `saveRDS(a, file = "data.rds")`, internally serialization
is used to encode the in-memory representation of `a` to a byte stream
that is saved to file `data.rds`. This object can then be read into
memory in another R session using `b <- readRDS("data.rds")`, which
internally relies on _unserialization_ to decode the byte stream to a
new in-memory, cloned representation of the object.  A similar
strategy can be used to communicate objects with another R process
running in parallel.

Some R objects are designed solely for the R process they were created
in. If we attempt to use them in another R process, they are not
longer valid.  For instance, an R connection `con` created by `con <-
file("somefile.txt", open = "wb")` is only valid in the current R
session. If we saved it to disk and read it back in another Rprocess
it will be a broken R connection.  The current inplementation of R may
or may not detect this is a broken. If we wrote to `con` in another R
connection, we risk writing to a completely different R connection and
destination.  Other types of objects might even crash R if used after
unserialization, e.g. those created by packages **bigmemory**,
**polars**, and **XML**.  This is because their objects comprise
external pointers that become null pointers when serialized and
unserialized, which these packages do not anticipate.

For some object types, there are workarounds to reconstruct them in
another R session through a technique referred to as
_marshalling_. Marshalling involves re-encoding the object before
serialization such that it can be re-constructed after
unserialization. For example, an **XML** object `node` can be
marshalled using `node_ <- XML::xmlSerializeHook(node)` and later be
unmarshalled using `node <- xmlDeserializeHook(node_)`.  However,
contrary to `serialize()` and `unserialize()`, there is no
well-established standard for marshalling and unmarshalling in R,
which means each user and developer has to be aware of the problem and
know which specialized functions to call to handle the problem, if at
all possible.  

## Goals  
This working group aims at developing standard practices for marshalling and unmarshalling of R objects.  This will
involve identifying current problems, raising awareness of it, coming
up with technical solutions, which might require additions to base
R. For example, one solution _might_ be to introduce support for
`serialize()` and `unserialize()` to call registered hook functions
whenever certain types of objects are encountered, which then could
marshall and unmarshall those objects.  

## Working Group Operations  
Our main modes of communication will be through issues and discussions on
a GitHub repository specifically created for the working group. We
will meet online for coordination as needed, initially with an aim to
meet at every two-to-three months.


## Team

* [Henrik Bengtsson](https://github.com/HenrikBengtsson) (Futureverse, R Consortium ISC, R Foundation)
* [Sebastian Fischer](https://github.com/sebffischer) (mlr3)
* ...
