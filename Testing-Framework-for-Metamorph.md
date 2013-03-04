Testing Metamorph definition files follows a simple pattern. Given the
respective definition and an input, a specific output is expected. This can
easily be expressed in XML:

```xml
<metamorph-test version="1.0"
	xmlns="http://www.culturegraph.org/metamorph-test" 
	xmlns:mm="http://www.culturegraph.org/metamorph"
	xmlns:cgxml="http://www.culturegraph.org/cgxml">
   <test-case name="My Testcase1">
      <input type="text/x-cg+xml">
	<!-- Your test input goes here -->           
      </input>
      <transformation type="text/x-metamorph+xml">
          <!-- the metamorph definition you want to test goes here  -->   
      </transformation>
      <result type="text/x-cg+xml">
          <!-- the expected result goes here  -->  
      </result>
   </test-case>
</metamorph-test>
```

How to integrate such a test definition written in XML into JUnit? JUnit feeds
on Java classes. Thus we need to provide such a class as a binding point:

```java
import org.culturegraph.metamorph.test.TestSuite;

@RunWith(TestSuite.class)
@TestDefinitions({"My Testcase1.xml", "My Testcase2.xml"})
public final class MyTest {/*bind to xml test*/}
```

The `RunWith` annotation instructs JUnit to let `org.culturegraph.metamorph.test.TestSuite` handle the testing.
Use the `TestDefinitions` annotation to tell `TestSuite` were to look
for tests. If no such annotation is found, `TestSuite` looks for an XML
files with the same name as the binding class. The XML files are expected to be
located in the same folder as the binding class. The rationale is that both
belong together and separating them would be confusing. Collocating the xml
files with the class files is causing trouble with same build environments
though. In the case of Maven there is an straight forward remedy. Add the following code to your POM: 

```xml
<testResources>
   <testResource>
     <directory>src/test/java</directory>
        <excludes><exclude>**/*.java</exclude></excludes>	
   </testResource>
   <testResource>
      <directory>src/test/resources</directory>
   </testResource>
</testResources> 
```

Here is an example test output in Eclipse:

![screenshot](img/junit.png)

The root
element is the binding class. Its children are the XML files, with the actual
tests as leafs.