This document provides an introduction to the Metafacture Morph language (short: Metamorph) for transforming metadata.

# Running Metamorph

# The Metamorph Definition

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

_to come soon_