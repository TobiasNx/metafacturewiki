Metafacture is a tool suite for metadata processing.


# Architecture
Metafacture comprises three main parts: Framework, Morph and Flux. It can be extended with modules.

## Framework
The framework includes the interfaces and abstract classes which form the foundation of the data processing pipelines. This part of Metafacture is only relevant for you if you plan to use Metafacture as a Java library or if you wish to add pipe elements to Flux.

For more information see the [[Framework User Guide]].

## Morph
Metafacture Morph or short Metamorph is a domain specific language for metadata transformation. The Metamorph object performing the transformation is used as part of a processing pipeline. If you are using Metafacture as a Java library, just create a `Metamorph` object and add it to your pipeline (see also the [[Framework User Guide]]). If you are using the Flux scripting language to build and run pipelines, use the `morph` command. 

The transformation itself is declared in XML. For more information on how to declare transformations see [[Metamorph Definition Language]].

The development of Metamorph definitions are supported by a few tools/techniques:
For more information on testing Metamorph definitions see [[Testing Framework for Metamorph]].
If you would like to visualize a definition as a data flow, see [[Visualizing Metamorph Definitions]].


## Flux

Flux is a scripting language to easily build and run processing pipelines. No Java programming is necessary, just a command line. To use Flux you may download the binary distribution of Metafacture:
_link to come soon_

For more information on how to use Flux, see the [[Flux User Guide]].

## Modules

Metafacture is extendable.

# Download

## As an Application

Download a snapshot of version [1.0.0](http://culturegraph.github.com/distributions/metafacture-core/metafacture-core-1.0.0-SNAPSHOT-bin.zip), [signature](http://culturegraph.github.com/distributions/metafacture-core/metafacture-core-1.0.0-SNAPSHOT-bin.zip.sig). 

_Stable release of 1.0.0 is comming soon_

## As Maven Dependency
Metafacture-core is released on [Maven Central](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22org.culturegraph%22).
To include it in a java project, add the following dependency declaration to your `pom.xml`:
```xml
<dependency>
    <groupId>org.culturegraph</groupId>
    <artifactId>metafacture-core</artifactId>
    <version>0.0.0</version>
</dependency>
```
Snapshot versions are available on [oss.sonatype.org](https://oss.sonatype.org/index.html#nexus-search;quick~culturegraph).

## As Source (Build your own)

Clone the repository: https://github.com/culturegraph/metafacture-core/
To build a distribution run `mvn clean package assembly:single`. You find the distribution as zip-file in the folder `target/`.


# Developer's Corner 
Our status: ![build status](https://travis-ci.org/culturegraph/metafacture-core.png?branch=master)

* [[Discussion on Flux syntax]]
* [[Code Quality and Style]]