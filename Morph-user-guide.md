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

