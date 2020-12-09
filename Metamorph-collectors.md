Fragem:
Was meint record hier? Den ganzen Datensatz?
Was meint name? _name_ , den Feldnamen?




In the case that an output depends on the values from more then one literal, we need to collect literals.
Collectors are defined under the <rules> tag, just as <data> tags. <data> tags are be put inside the respective collectors to indicate which literals are to be collected. The following paragraphs briefly introduce the different collectors available. Note that all types of collectors except <entity> can be nested. To all types of collectors post-processing steps can be added by using the `<postprocess>` tag.

de: Falls der Output auf Werte von mehr als einem Literal angewisen ist, werden Collector benötigt.
Collector... xml spezifisches
Hinweis: Alle collectors außer entity können verschachtelt werden. (?Stimmt das für FIX


For more information, see:
* [[Metamorph Cookbook]]
* test-cases (https://github.com/culturegraph/metafacture-core/tree/master/src/test/java/org/culturegraph/mf/morph/collectors)
* Metamorph schema (https://github.com/culturegraph/metafacture-core/blob/master/src/main/resources/schemata/metamorph.xsd).

## combine

`<combine>` is used to build one output literal from a combination of input literals. The following example for instance collects the sur- and forename which are stored in separate literals to combine them according to the pattern 'surname, forename'.

de:
`<combine>` wird genutzt um ein Output-Literal aus einer Kombination verschiedener Input-Literale nach einem Muster zu erzeugen. Das folgende Beispiel verbindet den Nach- und Vornamen, die in verschiedenen Literalen abgelegt wurden zu einem gemeinsamen Literal mit dem Muster "Nachname, Vorname"



```xml
<combine name="gnd:variantNameForThePerson" value="${surname}, ${forename}">

  <data source="028A.a" name="surname" />
  <data source="028A.d" name="forename"/>
</combine>
```
fix:
```
do combine('gnd:variantNameForThePerson', '${surname}, ${forename}')
  map(028A.a, surname)
  map(028A.d, forename)
end
```
There are several important points to note: By default <combine> waits until at least one value from each `<data>` tag is received. If the collection is not complete on record end, no output is generated. After each output, the state of `<combine>` is reset. If one `<data>` tag receives literals repeatedly before the collection is complete only the last value will be retained.

de:
Hinweise: Standardmäßig wartet `combine` darauf, dass zumindes ein Wert eingebetteten `map` weitergegeben wird. Wenn nicht alle eingebetteten `map`s Werte weitergeben, dann wird kein Output generiert. Nach jedem Durchlauf wird combin resettet. Wenn mehrere Literale durch ein Map empfangen werden, also mehrere Felder passend gemappt werden, dann wird nur der letzte Wert gesichert.


The standard behavior of `<combine>` can be controlled with several arguments: 
## flushWith
`flushWith= "name"` (fix: `flushWith: 'name'` ) generates output on the occurrence of any literal or entity with _name_. Variables in the output pattern which are not yet bound to a value, are replaced with the empty string. Use `flushWith="record"` to set the record end as output trigger. 
## reset
`reset="false"` (fix: `reset: 'false'`) disables the reset after output. 
## sameEntity
`sameEntity="true"` (fix: `sameEntity: 'true'`) will reset the `<combine> `after each entity end and thus enforce combinations stemming from the same entities only. Note that the implementation only executes a reset if actually needed. Using `sameEntity="true"` has thus no negative impact on performance.

de:
Das Standardverhalten von `combine` kann durch Argumente angepasst werden:
## flushWith (Verstehe ich nicht ganz)
`flushWith= "name"` (fix: `flushWith: 'name'` ) generiert Output beim Auftreten eines jeden Literals oder Objekt mit :_name_. Variablen im Outputmuster die noch nicht zu einem Wert verbunden sind, werden mit einem leeren String ersetzt. Nutze "flushWith: `record` um Output erst am Ende des Datensatzes zu triggern.

## reset
`reset="false"` (fix: `reset: 'false'`) deaktiviert das default-reset nach Output.

## sameEntity
`sameEntity="true"` (fix: `sameEntity: 'true'`) setzt combine nach jedem Objekt zurück und setzt so nur die Kombination der gleichen Entitäten durch. Ein Reset wird nur umgesetzt wenn wirklich notwenidg. Es gibt dirch  `sameEntity="true"` keine negativen Konsequenzen für die Performanz.

 
#   concat
collects all received values and concatenates them on record end. `flushWith="name"`
can be used to write the concatenation at the occurrence of any literal or entity _name_.

de:
Sammelt alle mit map empfangenen Werte und verkettet sie am Ende des Eintrag entsprechend eines Trenn-Strings.


```xml
in: ("data1", "a"), ("data2", "b"), ("data2", "c"),("data1", "d"),
<concat delimiter=", " name="concat" prefix="{" postfix="}">
   <data source="data1" />
   <data source="data2" />
</concat>
out: ("concat", "{a,b,c,d}")
```

fix:
```
in: ("data1", "a"), ("data2", "b"), ("data2", "c"),("data1", "d"),
do concat(delimiter: ',', name: 'concat', prefix: '{', postfix: '}')
   map(data1)
   map(data2)
end
out: ("concat", "{a,b,c,d}")
```

Note: The values are concatenated in the order they appear in the input and not in the order of the data sources.
Hinweise: Die Werte werden in der Reihenfolge verkettet, wie sie im Imput ankommen, nicht in der Reihenfolge der angegebenen map-Resourcen.


#   choose
collects all received values and emits the most preferred one on record end.  Preference is
assigned according to the order the data sources appear within the choose tag.  Eligible
arguments are `sameEntity` and `flushWith`.

sammelt alle Werte und gibt den bevorzugten Wert zurück. Präferenz werden entpsrechend der Reihenfolge Datenquelle/map bestimmt.

```xml
in: ("data1", "a"), ("data2", "b")
<choose name="data">
   <data source="data1" />
   <data source="data2" />
</choose>
out: ("data", "a")
```
fix:
```
in: ("data1", "a"), ("data2", "b")
do choose(name: 'data')
   map(data1)
   map(data2)
end
out: ("data", "a")
```


if, however `("data1", "a")` was missing, the output would be `("data", "b")`.


#   group
Use it to set name, value or both only once for an entire group of data or collect (combine,
choose, etc. ) tags.

Kann genutzt werden, um Namen, Werte oder beides nur einmal für eine komplette Gruppe zu setzen.
HIER FEHLEN BEISPIELE.

#    tuples (Verstehe ich nicht?)
collects literals from the enclosed sources and emits all possible combinations of them when
flushed.  The `minN` option is used to set a minimum of different sources to be received.  If
less than `minN` are received no tuples are emitted.



# square
All unordered 2-tuples are constructed. Öffnet Tuples
```xml
in: ("data", "a"), ("data", "b"), ("data", "c")
<square delimiter="," name="square" prefix="{" postfix="}">
   <data source="data" />
</square>
out: ("square", "{a,b}"), ("square", "{a,c}"),("square", "{b,c}")
```
fix:
```
in: ("data", "a"), ("data", "b"), ("data", "c")
do square(delimiter:',', name: 'square', prefix: '{', postfix: '}')
   map(data)
end
out: ("square", "{a,b}"), ("square", "{a,c}"),("square", "{b,c}")

```

#   Entity
collects literals to rearrange them as an entity. Use the argument name to assign a name to
the entity. Further arguments are `sameEntity`, `flushWith` and `reset`. Note that the `<entity>`
tag can only appear as child of the `<rules>` tag or another entity tag as it does not output
a literal.

fix: `do entity`

Sammelt Literale, um sie als ein Objekt zusammenzubringen. Man kann dem Objekt mit einem argument name einen Name zuweisen. Zusatzlich gibt es e `sameEntity`, `flushWith` and `reset` als Argumente. Kann nicht in andere Collectoren eingebettet werden. AUßER Array.


# Array

Erzeugt aus gesammelten Literalen einen Array. Entity kann in `do array` eingebettet werden.
