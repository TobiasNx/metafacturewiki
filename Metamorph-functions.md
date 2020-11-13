Functions take as input name-value pairs, subsequently written as `(name,  value)`
and output zero to n name-value pairs.

For more information, see also:
* [[Metamorph Cookbook]]
* [Test cases](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/test/java/org/metafacture/metamorph/functions)
* [All functions](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/resources/morph-functions.properties)
* [Metamorph schema](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/resources/schemata/metamorph.xsd)

# compose
Wraps the value in a prefix and postfix.
```xml
in:      ("a","b")("c","d")  ...
<compose  prefix="Hello,  "  postfix="!"/>
out:   ("a","Hello  b!")("c","Hello  d!")  ...
```
fix: 
```
in:      ("a","b")("c","d")  ...
compose(prefix: 'Hello' postfix: '!')
out:   ("a","Hello  b!")("c","Hello  d!")  ...
```

#    constant
Replaces the value with a constant string.
```xml
in:      ("a","b")("c","d")  ...
<constant  value="V"/>
out:   ("a","V")("c","V")  ...
```
fix: 
```
in:      ("a","b")("c","d")  ...
constant('V')
out:   ("a","V")("c","V")  ...
```

#    count
Counts occurrences.
```xml
in:      ("a","b")("c","d")  ...
<count/>
out:   ("a","1")("c","2")  ...
```
fix:

_needs to be added later_

#    regexp
Regexp matching. Returns first occurrence of a pattern. The pattern is a Java regex Pattern
(see http://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html).

```xml
in:      ("a","baum")("b","pflaume")("d","apfel")  ...
<regexp  match="a.m"/>
out:   ("a","baum")("b","pflaume")  ...
```

fix: 
```
in:      ("a","baum")("b","pflaume")("d","apfel")  ...
regexp(match:'a.m')
out:   ("a","baum")("b","pflaume")  ...
```
The optional format argument is used to address match groups.

```xml
in:      ("a","from  1789  to  1900")  ...
<regexp  match="(\d\d\d\d)  to  (\d\d\d\d)"  format="${1}-${2}"/>
out:   ("b","1789-1900")  ...
```
fix: 
```
in:      ("a","from  1789  to  1900")  ...
regexp(match:'(\d\d\d\d)  to  (\d\d\d\d)', format: '${1}-${2}')
out:   ("b","1789-1900")  ..
```
#    replace
Replaces a pattern with a string. The pattern is a Java regex Pattern (see http://docs.
oracle.com/javase/6/docs/api/java/util/regex/Pattern.html).

```xml
in:      ("a","abcde")("c","efg")  ...
<replace  pattern="[ace]"  with="X"/>
out:   ("a","XbXdX")("c","Xfg")  ...
```
fix: 
```
in:      ("a","abcde")("c","efg")  ...
replace_all(pattern: '[ace]',  with: 'X')
out:   ("a","XbXdX")("c","Xfg")  ...
```
#    setreplace
Replaces multiple strings.  The mapping from string to replacement is defined in a map,
just as done in the lookup function. See also [[Data Lookup]].
```xml
in:      ("a","written  in  $en")("c","$es-speaking")  ...
<setreplace>
<entry  name="$en"  value="english"/>
<entry  name="$es"  value="spanish"/>
</setreplace>
out:   ("a","written  in  english")("c","spanish-speaking")  ...
```
fix:

_needs to be added later_

#    substring
Extraction of a substring.
```xml
in:      ("a","012345")  ...
<substring  start="3"  end="5"/>
out:   ("a","34")  ...
```
fix: 
```
in:      ("a","012345")  ...
substring(start: '3',  end: '5')
out:   ("a","34")  ...
```


#   lookup
Lookup and replace based on a data table. See also [[Data Lookup]].
```xml
in:      ("a","en")("b","es")("c","xy")  ...
<lookup>
<entry  name="en"  value="english"/>
<entry  name="es"  value="spanish"/>
</lookup>
out:   ("a","english")("c","spanish")  ...
```
Fix:
no specific entry :
```
in:      ("a","en")("b","es")("c","xy")  ...
lookup(in: '[source of mapping list].tsv')
out:   ("a","english")("c","spanish")  ...
```

#    whitelist
Filtering based on a whitelist. See also [[Data Lookup]]
```xml
in:      ("a","rabbit")("b","turtle")("c","hamster")  ...
<whitelist>
<entry  name="hamster"/>
<entry  name="rabbit"/>
</whitelist>
out:   ("a","rabbit")("c","hamster")  ...
```
fix:

_needs to be added later_

#    blacklist
Filtering based on a blacklist. See also [[Data Lookup]].
```xml
in:      ("a","rabbit")("b","turtle")("c","hamster")  ...
<blacklist>
<entry  name="hamster"/>
<entry  name="rabbit"/>
</blacklist>
out:   ("b","turtle")  ...
```
fix:

_needs to be added later_

#    isbn
ISBN cleaning, checkdigit verification and transformation between ISBN 10 and ISBN 13.

#    equals
Filtering based on equality.
```xml
in:      ("a","hamster")("b","turtle")  ...
<equals  string="hamster"/>
out:   ("a","hamster")  ...
```
fix: 
```
in:      ("a","hamster")("b","turtle")  ...
equals(string: 'hamster')
out:   ("a","hamster")  ...
```
#    not-equals
Filtering based on inequality.
```xml
in:      ("a","hamster")("b","turtle")  ...
<not-equals  string="hamster"/>
out:   ("b","turtle")  ...
```
fix: 
```
in:      ("a","hamster")("b","turtle")  ...
not_equals(string: 'hamster')
out:   ("b","turtle")  ...
```
#    contains
Filtering based on containing.
```xml
in:      ("a","hamster in the house")("b","turtle in the house")  ...
<contains  string="hamster"/>
out:   ("a","hamster in the house")  ...
```

fix: 
```
in:      ("a","hamster in the house")("b","turtle in the house")  ...
contains(string: 'hamster')
out:   ("a","hamster in the house")  ...
```
#    not-contains
Filtering based on not containing.
```xml
in:      ("a","hamster in the house")("b","turtle in the house")  ...
<not-contains  string="hamster"/>
out:   ("b","turtle in the house")  ...
```
fix: 
```
in:      ("a","hamster in the house")("b","turtle in the house")  ...
not_contains(string: 'hamster')
out:   ("b","turtle in the house")  ...
```
#    htmlanchor
create an HTML anchor tag.

#    trim
Trim the value.
```xml
in:      ("a","  hamster  ")("b","turtle  ")  ...
<trim  string="hamster"/>
out:   ("a","hamster")("b","turtle")  ...
```
fix: 
```
in:      ("a","  hamster  ")("b","turtle  ")  ...
trim()
out:   ("a","hamster")("b","turtle")  ...
```

#    split
splitting based on a regexp.

```xml
in:      ("data","Oahu,Hawaii,Maui")  ...
<split delimiter=","/>
out:   ("data","Oahu")("data","Hawaii")("data","Maui")  ...
```
fix: 
```
in:      ("data","Oahu,Hawaii,Maui")  ...
split(delimiter: ',')
out:   ("data","Oahu")("data","Hawaii")("data","Maui")  ...
```

#    script and java
processing the value with a JavaScript function or a Java class. See [[Integration of Java and JavaScript]].

# switch-name-value
Exchanges name and value.

```xml
in:      ("hamster","Rodriguez")("cat","Felix")  ...
<switch-name-value/>
out:   ("Rodriguez","hamster")("Felix","cat")  ...
```
fix:

_needs to be added later_

If you wish to set the name of the resulting named values, use the following code:
```xml
in:      ("hamster","Rodriguez")("cat","Felix")  ...
<constant value="animal"/>
<switch-name-value/>
out:   ("animal","hamster")("animal","cat")  ...
```
fix:

_needs to be added later_

#    normalize-utf8
UTF-8 normalization. Brings Umlauts into canonical form.
#    occurrence
filtering based on occurrence.
```xml
in:      ("a","hamster")("b","turtle")("c","butterfly")  ...
< occurrence  only="2"/>
out:   ("b","turtle")  ...
```
fix: 
```
in:      ("a","hamster")("b","turtle")("c","butterfly")  ...
occurrence(only: '2')
out:   ("b","turtle")  ...
```
```xml
in:      ("a","hamster")("b","turtle")("c","butterfly")  ...
< occurrence  only="moreThan  2"/>
out:   ("c","butterfly")  ...
```
fix: 
```
in:      ("a","hamster")("b","turtle")("c","butterfly")  ...
occurrence(only: 'moreThan  2')
out:   ("c","butterfly")  ...
```