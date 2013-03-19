In case the predefined functions for data processing be not suffiecient, two
choices to extend Metamorph scripts with customized code are at disposal: The
first is to write a Java class. The second is to load a JavaScript file.

# Java

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

# JavaScript

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