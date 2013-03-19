

A certain group of functions takes a map/dictionary as argument: `<lookup>`, `<whitelist>`, `<blacklist>` etc.
In this section the usage of such maps will be explained. We start with a simple example of data lookup.

# Local Lookup

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

# Maps

The same lookup tables may used in different places in a Metamorph definition. To enable reuse, a map/dictionary can be defined separately from the respective lookup function. In the following listing the `<lookup>` function refers to the table using the name _material_. Later in the code the actual map is defined using the `<map>` tag. See listing \ref{maps}.

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

# External Data Sources

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