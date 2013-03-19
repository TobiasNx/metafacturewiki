Functions take as input name-value pairs, subsequently written as `(name,  value)`
and output zero to n name-value pairs.
# compose
Wraps the value in a prefix and postfix.
```xml
in:      ("a","b")("c","d")  ...
<compose  prefix="Hello  "  postfix="!"/>
out:   ("a","Hello  a!")("c","Hello  d!")  ...
```
#    constant
Replaces the value with a constant string.
```xml
in:      ("a","b")("c","d")  ...
<constant  value="V"/>
out:   ("a","V")("c","V")  ...
```
#    count
Counts occurrences.
```xml
in:      ("a","b")("c","d")  ...
<count/>
out:   ("a","1")("c","2")  ...
```
#    regexp
Regexp matching. Returns first occurrence of a pattern. The pattern is a Java regex Pattern
(see http://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html).

```xml
in:      ("a","baum")("b","pflaume")("d","apfel")  ...
<regexp  pattern="a.m"/>
out:   ("b","aum")("c","aum")  ...
```

The optional format argument is used to address match groups.

```
in:      ("a","from  1789  to  1900")  ...
<regexp  pattern="(\d\d\d\d)  to  (\d\d\d\d)"  format="${1}-${2}"/>
out:   ("b","1789-1900")  ...
```xml

#    replace
Replaces a pattern with a string. The pattern is a Java regex Pattern (see http://docs.
oracle.com/javase/6/docs/api/java/util/regex/Pattern.html).

```xml
in:      ("a","abcde")("c","efg")  ...
<replace  pattern="[ace]"  with="X"/>
out:   ("a","XbXdX")("c","Xfg")  ...
```

#    setreplace
Replaces multiple strings.  The mapping from string to replacement is defined in a map,
just as done in the lookup function. See also [[Data Lookup]].
```xml
in:      ("a","written  in  $en")("c","$es-speaking")  ...
<setreplace>
<entry  name="$en"  value="english"/>
<entry  name="$es"  value="spanish"/>
</setreplace>
out:   ("a","written  in  english")("c","spanish-speaking")  ...
```
#    substring
Extraction of a substring.
```xml
in:      ("a","12345")  ...
<substring  start="3"  end="5"/>
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
#    whitelist
Filtering based on a whitelist. See also [[Data Lookup]]
```xml
in:      ("a","rabbit")("c","turtle")("c","hamster")  ...
<whitelist>
<entry  name="hamster"/>
<entry  name="rabbit"/>
</whitelist>
out:   ("a","rabbit")("c","hamster")  ...
```

#    blacklist
Filtering based on a blacklist. See also [[Data Lookup]].
```xml
in:      ("a","rabbit")("c","turtle")("c","hamster")  ...
<blacklist>
<entry  name="hamster"/>
<entry  name="rabbit"/>
</blacklist>
out:   ("b","turtle")  ...
```
#    isbn
ISBN cleaning, checkdigit verication and transformation between ISBN 10 and ISBN 13.

#    equals
Filtering based on equality.
```xml
in:      ("a","hamster")("b","turtle")  ...
<equals  string="hamster"/>
out:   ("a","hamster")  ...
```
#    not-equals
Filtering based on inequality.
```xml
in:      ("a","hamster")("b","turtle")  ...
<not-equals  string="hamster"/>
out:   ("b","turtle")  ...
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
#    split
splitting based on a regexp.

#    script and java
processing the value with a JavaScript function or a Java class. See [[Integration of Java and JavaScript]].

# switch-name-value
Exchanges name and value.

```xml
in:      ("hamster","Rodriguez")("cat","Felix")  ...
<switch-name-value/>
out:   ("Rodriguez","hamster")("Felix","turtle")  ...
```
If you wish to set the name of the resulting named values, use the following code:
```xml
in:      ("hamster","Rodriguez")("cat","Felix")  ...
<contant value="animal">
<switch-name-value/>
out:   ("animal","hamster")("animal","cat")  ...
```

#    normalize-utf8
UTF-8 normalization. Brings Umlauts into canonical form.
#    occurrence
filtering based on occurrence.
```xml
in:      ("a","hamster")("b","turtle")("c","butterfly")  ...
< occurrence  only="2"/>
out:   ("b","turtle")  ...
```
```xml
in:      ("a","hamster")("b","turtle")("c","butterfly")  ...
< occurrence  only="moreThan  2"/>
out:   ("b","turtle")("c","butterfly")  ...
```