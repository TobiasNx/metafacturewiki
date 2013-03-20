Please add recipes under 'Solutions'. If you have a problem in need of a solution, please add it under 'Open Problems'.

# Solutions

The following sections present solutions to common problems.

## Emitting value of A if A occurs, but 'x' if A is missing
Use `<choose>` to give preference to A. Add a data source for '_id' as fallback (the record id literal '_id' is guaranteed to occur in every record).

```xml
<choose name="out">
   <data source="A"/>
   <data source="_id">
      <constant value="x">
   </data>
</combine>
```
A variation is to emit for every entity E the value of A if A occurs, but 'x' if A is missing in E:
```xml
<choose name="out" flushWith="E">
   <data source="E.A"/>
   <data source="E">
      <constant value="x">
   </data>
</combine>
```
In this case the fallback is E. Note that `<choose>` needs to be flushed with every occurrence of E.

## Emitting value of A whenever B occurs

If A happens only once and before Bs:

```xml
<combine name="" value="${a}" reset="false">
   <data source="A" name="a"/>
   <data source="B" name=""/>
</combine>
```
Note that `reset` is set to `false` in order to retain the value of A.

If A happens only once but after Bs, the Bs must be delayed by buffering them:
```xml
<combine name="" value="${a}" reset="false">
   <data source="A" name="a"/>
   <data source="B" name="">
      <buffer/>   
   </data>
</combine>
```

## Avoid reset of a single value in a collector

This is a variation of 'Emitting value of A whenever B occurs'.

Imagine a record which stores all variant names of a person in the entity '028@'. We want to combine the name with the person id which occurs only once per record in '003@.0'. We need to flush the collector to avoid the mixing of name parts between different name, but would like to retain the id.
The solution is:
 
```xml
<combine name="name"
        value="${surname}${forename}, ${id}"
        flushWith="028@" reset="true">
    
    <combine name="id" value="${value}" reset='false'>
      <data source="028@"/>
      <data source="003@.0" name="value"/> 
    </combine>
      
    <data source="028@.a" name="surname"/>  
    <data source="028@.d" name="forename"/>  
 </combine>  
```
The id is re-emitted for every entity '028@'. The `reset=false` in the inner `<combine>` assures that the id is retained for the next occurrence of entity '028@'.




# Open Problems



The first `<combine>` element use the name “@pid” to declare a redirecting the value return to the input stream and it is done for each new 028@. The second `<combine>` element uses the redirected, but not reset value of 003@ to combine the new 028@ values which are reset before read.