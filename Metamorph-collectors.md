In the case that an output depends on the values from more then one literal, we need to collect literals. 
This page lists the available collectors. For more information, see the test-cases (https://github.com/culturegraph/metafacture-core/tree/master/src/test/java/org/culturegraph/mf/morph/collectors) and the Metamorph schema (https://github.com/culturegraph/metafacture-core/tree/master/src/main/resources/schema/metamorph.xsd).

#   Concatenate
collects all received values and concatenates them on record end. `flushOn=  "entityname"`
can be used to concatenate at the end of entity _entityname_.

#   Choose
collects all received values and emits the most preferred one on record end.  Preference is
assigned according to the order the data sources appear within the choose tag.  Eligible
arguments are `sameEntity` and `flushOn`.

#   Group
Use it to set name, value or both only once for an entire group of data or collect (combine,
choose, etc. ) tags.

#    Tuples
collects literals from the enclosed sources and emits all possible combinations of them when
flushed.  The `minN` option is used to set a minimum of different sources to be received.  If
less than `minN` are received no tuples are emitted.

#   Entity
collects literals to rearrange them as an entity. Use the argument name to assign a name to
the entity. Further arguments are sameEntity, flushOn and reset. Note that the `<entity>`
tag can only appear as child of the `<rules>` tag or another entity tag as it does not output
a literal.