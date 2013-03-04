Metafacture is a tool suite for metadata processing.

# Motivation

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


# Developer's Corner

* [[Discussion on Flux syntax]]
* [[Code Quality and Style]]
