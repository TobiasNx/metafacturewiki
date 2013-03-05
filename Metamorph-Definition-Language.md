This document provides an introduction to the Metafacture Morph language (short: Metamorph) for transforming metadata.

The transformation a specific Metamoph instance performs are defined in a Metamorph definition in XML. The following code snippet shows the high level organization of a Metamorph definition. Its structure of the XML is constrained by the schema `metamorph.xsd`.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metamorph xmlns="http://www.culturegraph.org/metamorph"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.culturegraph.org/metamorph metamorph.xsd"
	entityMarker="." version="2">
   <meta><!-- Metadata --></meta>
   <macros><!-- Macro definitions --></macros>
   <rules><!-- Transformation rules --></rules>
   <maps><!-- Data maps --></maps>
</metamorph>
```
 

The root element `<metamorph>` has two attributes: One indicates the Metamorph version the document it intended to work with, the second indicates the character used to separate entity names. Within the `<metamorph>` tag there are four sections.
The first and optional one holds metadata for the definition file. The second section -- also optional -- holds definition of macros.
The `<rules>` block defines the actual transformation rules. 
Finally the optional `maps` block allows to define maps/dictionaries for lookup functionality.

## Addressing Pieces of Data

The `<data>` tag is used to receive literals. Use the `source` attribute to address the literal you want to catch. The following code would receive the value of any literal with name _literalname_, enclosed in an entity named _entityname_.

```xml
<data source="entityname.literalname" name="newName" />
```

The value is then sent to the downstream data flow element under the name _newName_. It is thus the most basic form of mapping data. 


The `source` attribute also accepts wildcards. For instance the star-wildcard: `<data source="person*" />` would match all literals with names starting with 'person': 'person\_name', 'person\_age', etc.
Apart from the star-wildcard, the questionmark-wildcard ('?') is supported. It matches exactly one arbitrary character.

Finally, sources can be concatenated using the pipe-character ('|') to express a logical-or relationship: `<data source="creator|contributor" />` would match both 'creator' and 'contributor'. Please note that the pipe connects complete source names. It does not apply to parts or characters.

## Processing Pieces of Data
After picking up a literal, its content can be processed.
Processing steps are added inside the `<data>` tag. The following code shows an example in which the date of death of an author in the PND is extracted from the Pica records and renamed to the corresponding RDF property (for the complete mapping description see the DNB linked data service documentation).
```xml
<data name="rdaGr2:dateOfDeath" source="032Aa.a">
  <replace pattern=" " with="" />
  <regexp match="-((\d*?))$" format="${1}" />
</data>
```

In the PND birth and death of an author are stored both in one subfield (literal in Metamorph speak) in the form 'birth - death' . So the need for processing arises.
First we eliminate all whitespaces by using a `<replace>` operation. Next we apply regular expression matching `<regexp>` and extract the firs match group (${1}) corresponding to the year of death.

Please note that functions may return zero to n values. If no value is returned, the processing is stopped and nothing will be sent downstream. If for instance a `<regexp>` does not match, processing stops and there will be no 'rdaGr2:dateOfDeath' in the output stream.

For a list of available functions see [[Metamorph Functions]] or the xml schema `metamorph.xsd`.

## Looking up  Pieces of Data

A certain group of functions takes a map/dictionary as argument: `<lookup>`, `<whitelist>`, `<blacklist>` etc.
In this section the usage of such maps will be explained. We start with a simple example of data lookup.

### Local Lookup

Take for instance an operation in which you want to replace values according to a lookup table: Value 'A' maps to 'print', 'B' maps to 'audiovisual' an so forth. This is accomplished by the `<lookup>` function. The lookup table is defined inside the `<lookup>` tag. The following code snippet depicts this situation.

```xml
<data source="002@.0" name="dcterms:format">
  <substring start="0" end="1" />
  <lookup>
    <entry name="A" value="print" />
    <entry name="B" value="audiovisual" />
    <entry name="O" value="online" />
  </lookup>
</data>	
```

### Maps

The same lookup tables may used in different places in a Metamorph definition. To enable reuse, a map/dictionary can be defined separately from the respective lookup function. In the following listing the `<lookup>` function refers to the table using the name _material_. Later in the code the actual map is defined using the {\tt map} tag. See listing \ref{maps}.

```xml
[...]
<lookup in="material">
[...]
<map name="material">
  <entry name="A" value="print" />
  <entry name="B" value="audiovisual" />
  <entry name="O" value="online" />
</map>
```

### External Data Sources

The situation might arise that the data used in lookup operations cannot be hardcoded in xml; or at least hard-coding it would be inconvenient. Imagine we want to resolve author ids to author names: Putting all the id-name mappings into the Metamorph definition file is certainly not desirable.
To address this issue, any data source implementing the `Map` interface can be connected to the `Metamorph` object as shown in the following code snippet. The data is referenced in the Metamorph definition file by 'name of map'.

```java
//create a Map. Any object implementing Map<String, String> will do
final Map<String, String> map = new HashMap<String, String>();
map.put("one key", "first String");
map.put("another key", "another String");

//tell metamorph to use it during lookup operations
metamorph.putMap("name of map", map);
```

## Integration of customized code

In case the predefined functions for data processing be not suffiecient, two
choices to extend Metamorph scripts with customized code are at disposal: The
first is to write a Java class. The second is to load a JavaScript file.

### Java

You may use Java classes as function using the `<java>` tag: 

```xml
<data source="data">
	<java class="org.culturegraph.metamorph.core.functions.Compose" prefix="Hula " />
</data>
```

The class must implement the `Function` interface. For each attribute
provided in the `<java>` tag, a respective `set` method is called
right after instantiation. This mechanism correctly handles the types `String`, `boolean` and `int`. A separate class is instantiated for
each use in Metamorph definition. A `Function` may thus maintain a state. See the
JavaDoc and code for more details.

### JavaScript

To invoke a JavaScript function use the `<script>` tag as shown in the following listing:
 
```xml
<data source="X">
   <script file="script.js" invoke="emphasize" />
</data>
```

The `<script>` takes as arguments the path to a JavaScript file and the name
of the function to be invoked using the data as sole argument. The return value
may be any object as Metamorph will call `toString()` on it before
proceeding. The following snipped shows the file `script.js`:

```javascript
function emphasize(val){
	return "<em>" + val + "</em>";
}
```

## Recursion

Pieces of data processed with Metamorph are by default sent to the `StreamReceiver` registered with Metamorph. However, there is the possibility to
send a piece of data into a feedback loop. In this case the data reenters
Metamorph just as it came from the upstream `StreamSender`. This recursion
is accomplished by prepending an '@' to the name of the data:

```xml
<data source="002@.0" name="@format">
   <!--  processing -->
</data>

<!--  catch the data -->
<data source="@format" name="dcterms:format">
```

This pattern comes in handy when a piece of data is needed at several other
places after preprocessing. It relieves you from copying and pasting the same
preprocessing steps. It also improves efficiency as Metamorph will perform the
preprocessing only once. Be careful though not to build infinite loops by
forgetting to rename the data (removing the '@') in the final processing step.

## Collecting Pieces of Data

In the case that an output depends on the values from more then one literal, we
need to collect literals. Collectors are defined under the `<rules>` tag, just
as `<data>` tags. `<data>` tags are be put inside the respective collectors
to indicate which literals are to be collected. The following paragraphs briefly
introduce the different collectors available. Note that all types of collectors
except `<entity>` can be nested. To all types of collectors post-processing steps can be added
by using the `<postprocess>` tag.


`<combine>` for instance is used to build one output literal from a
combination of input literals. The following example  for
instance collects the sur- and forename which are stored in separate literals to
combine them according to the pattern 'surname, forename'.

```xml
<combine name="gnd:variantNameForThePerson" value="${surname}, ${forename}">
  <data source="028A.a" name="surname" />
  <data source="028A.d" name="forename"/>
</combine>
```

There are several important points to note: By default `<combine>` waits until
at least one value from each `<data>` tag is received. If the collection is
not complete on record end, no output is generated. After each output, the state
of `<combine>` is reset. If one `<data>` tag receives literals repeatedly
before the collection is complete only the last value will be retained.



The standard behavior of `<combine>` can be controlled with several arguments:
`flushOn= "entityname"` generates output on the end of each entity with name
_entityname_. Variables in the output pattern which are not yet bound to a
value, are replaced with the empty string. Use `flushOn="record"` to set the
record end as output trigger.
`reset="false"` disables the reset after output.
`sameEntity="true"` will reset the `<combine>` after each entity end and
thus enforce combinations stemming from the same entities only. Note that the
implementation only executes a reset if actually needed. Using `sameEntity="true"` has thus no negative impact on performance.

For a list of all available collectors see [[Metamorph Collectors]]

## Parameters to Metamorph Definitions

Metamorph definitions may contain parameters. They follow the pattern `$[NAME]`:

```xml
<data name="edm:rights" source="_id">
   <constant value="$[rights]" />
</data>
```

`$[rights]` in this case is a compile-time variable which is evaluated on
creation of the respective Metamorph object.
Thes variable in square brackets are not to be confused with the ones in curly
brackets, which are evaluated at run-time.

Compile-time variable are passed to Metamorph as a constructor parameter.

```java
final Map<String, String> vars = new HashMap<String, String>();
vars.put("rights", "CC-0");

final Metamorph metamorph = new Metamorph("morphdef.xml", vars);
```

The `<vars>` section in the Metamorph definition can be used to set defaults:

```xml
<vars>
   <var name="rights" value="CC0" />
</vars>
```

## Defining Macros

Macros can be defined within the `<macros>` tag and use the same parameter
mechanism as code within the `<rules>` tag. 
Macros are called with the `<call-macro>` tag. Attributes
of the tag are used as parameters:

```xml
<macros>
   <macro name="concat-up">
      <concat delimiter=", " name="$[literal_name]">
         <data source="$[literal_name]" >
            <case to="upper"/>
         </data>
      </concat>
   </macro>
</macros>
<rules>
   <call-macro name="concat-up" literal_name="data1"/>
   <call-macro name="concat-up" literal_name="data2"/>
</rules>
```

Parameters are scoped, which means that the ones provided with the `call-macro` tag shadow global ones. Macros cannot be nested.



##Splitting Metamorph Definitions for Reuse

In a complex project setting there may be several Metamorph definitions in use,
and it is likely that they share common parts. Imagine for instance a
transformations from Marc 21 record holding data on books to RDF, and Marc 21
records hodling data on authors to RDF. Both make use of a table assinging
country names to ISO country codes. Such a table should only exist once. To
accomodate for such reuse, Metamorph offes an include mechanism based on
XInclude (http://www.w3.org/TR/xinclude/). The following snippet shows an example in which a `<map>` is included.

```xml
<!-- main metamorph definition -->				
[...]
<maps>
   <include href="src/test/resources/mymap.xml" parse="xml"
	 xmlns="http://www.w3.org/2001/XInclude" />
</maps>
[...]
```

```xml
<!-- mymap.xml -->
<?xml version="1.1" encoding="UTF-8"?>
<map name="island_map" xmlns="http://www.culturegraph.org/metamorph">
	<entry name="Aloha" value="Hawaii" />
</map>
```

Use the `<include>` tag from the http://www.w3.org/2001/XInclude
namespace to insert an external XML file into your definition. The included file
must be valid xml itself, containing syntactically valid tags from the Metamorph
namespace.

_to come soon_