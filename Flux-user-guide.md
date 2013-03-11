This document provides a quick introduction to Metafacture Flux, a domain specific language to build data flows for metadata processing.

# Installing Flux

There are two options: either you check out the source code in your favorite IDE, or you unzip a distribution of Metafacture into a directory of your choice. A distribution can be downloaded here: http://culturegraph.github.com

# Running Flux

If you are working with the source code directly, execute the class `org.culturegraph.mf.Flux`. If you are working with a distribution of Metafacture, execute the script `flux.sh` or `flux.bat` in the `bin/` folder.

## Getting Help and Inspiration
1. If executed without arguments, Flux will display a short help text along with a list of all registered commands. 
2. There are several example flux files along with sample data in the folder `examples/`: https://github.com/culturegraph/metafacture-core/tree/master/examples

## Run a Flux-File

Just provide the flux file you wish to run as first argument.

```bash
flux.sh FILE.flux
```

## Provide Arguments
To provide arguments add variable assignments after the first argument as follows:
```bash
flux.sh FILE.flux var1=value1 var2=value2
```
This sets the variable `var1` to the value 'value1' and `var2` to the value 'value2'.

# Writing Flux files

_under construction_

# Adding new Commands

_under construction_
