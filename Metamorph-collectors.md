In the case that an output depends on the values from more then one literal, we need to collect literals.
Collectors are defined under the <rules> tag, just as <data> tags. <data> tags are be put inside the respective collectors to indicate which literals are to be collected. The following paragraphs briefly introduce the different collectors available. Note that all types of collectors except <entity> can be nested. To all types of collectors post-processing steps can be added by using the `<postprocess>` tag.

For more information, see the test-cases (https://github.com/culturegraph/metafacture-core/tree/master/src/test/java/org/culturegraph/mf/morph/collectors) and the Metamorph schema (https://github.com/culturegraph/metafacture-core/tree/master/src/main/resources/schema/metamorph.xsd).

#combine

`<combine>` is used to build one output literal from a combination of input literals. The following example for instance collects the sur- and forename which are stored in separate literals to combine them according to the pattern 'surname, forename'.
```xml
<combine name="gnd:variantNameForThePerson" value="${surname}, ${forename}">
  <data source="028A.a" name="surname" />
  <data source="028A.d" name="forename"/>
</combine>
```
There are several important points to note: By default <combine> waits until at least one value from each `<data>` tag is received. If the collection is not complete on record end, no output is generated. After each output, the state of `<combine>` is reset. If one `<data>` tag receives literals repeatedly before the collection is complete only the last value will be retained.

The standard behavior of `<combine>` can be controlled with several arguments: 
## flushWith
`flushWith= "name"` generates output on the occurrence of any literal or entity with _name_. Variables in the output pattern which are not yet bound to a value, are replaced with the empty string. Use `flushOn="record"` to set the record end as output trigger. 
##reset
`reset="false"` disables the reset after output. 
##sameEntity
`sameEntity="true"` will reset the `<combine> `after each entity end and thus enforce combinations stemming from the same entities only. Note that the implementation only executes a reset if actually needed. Using `sameEntity="true"` has thus no negative impact on performance.

 
#   Concatenate
collects all received values and concatenates them on record end. `flushWith="name"`
can be used to write the concatenation at the occurrence of any literal or entity _name_.

#   Choose
collects all received values and emits the most preferred one on record end.  Preference is
assigned according to the order the data sources appear within the choose tag.  Eligible
arguments are `sameEntity` and `flushWith`.

#   Group
Use it to set name, value or both only once for an entire group of data or collect (combine,
choose, etc. ) tags.

#    Tuples
collects literals from the enclosed sources and emits all possible combinations of them when
flushed.  The `minN` option is used to set a minimum of different sources to be received.  If
less than `minN` are received no tuples are emitted.

# Square
All unordered 2-tuples are constructed.
```xml
in: ("data", "a"), ("data", "b"), ("data", "c")
<square delimiter="," name="square" prefix="{" postfix="}">
   <data source="data" />
</square>
out: ("square", "{a,b}"), ("square", "{a,c}"),("square", "{b,c}")

```

#   Entity
collects literals to rearrange them as an entity. Use the argument name to assign a name to
the entity. Further arguments are `sameEntity`, `flushWith` and `reset`. Note that the `<entity>`
tag can only appear as child of the `<rules>` tag or another entity tag as it does not output
a literal.