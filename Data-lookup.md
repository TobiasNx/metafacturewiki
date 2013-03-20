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

# Defining Maps

The same lookup tables may used in different places in a Metamorph definition. To enable reuse, a map/dictionary can be defined separately from the respective lookup function. 
In the following listing the `<lookup>` function refers to the table using the name _material_.
```xml
<lookup in="material">
```
To define a map, there are mainly three options (more to come in the future).

## Maps in XML

 With the `<map>` tag the contents of the map can be defined right away in the Metamorph definition.

```xml
<map name="material">
  <entry name="A" value="print" />
  <entry name="B" value="audiovisual" />
  <entry name="O" value="online" />
</map>
```

## Maps in separate text files
Sometimes, it is more convenient to store mappings in a separate text file:
```xml
<filemap name="map1" files="maps/MARC-country-codes.txt" separator="\t"/>
```

## Maps implemented by a Java class

The situation might arise that the data cannot be hard-coded in xml/text etc; or at least hard-coding it would be very inconvenient. Imagine we want to resolve author ids (> 5 Million) to author names: Putting all the id-name mappings into the Metamorph definition file or text file is certainly _not_ desirable.
To address this issue, Metamorph allows you to load any class which implements the `Map` interface.

```xml
<javamap name="map-name" class="org.myorginzation.MyMap" files="maps/MARCcountries.txt" />
```

`Map`s can also be added to Metamorph problematically in Java:

```java
//create a Map. Any object implementing Map<String, String> will do
final Map<String, String> map = new HashMap<String, String>();
map.put("one key", "first String");
map.put("another key", "another String");

//tell metamorph to use it during lookup operations
metamorph.putMap("name of map", map);
```

__Important:__ If your `Map` implementation allocates resources which need to be closed eventually (database connections etc.), let it implement the `java.io.Closeable` interface. Metamorph keeps track of all `Closable`s and will call the `close()` method upon `closeStream()`.