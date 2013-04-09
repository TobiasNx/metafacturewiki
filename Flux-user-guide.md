This document provides a quick introduction to Metafacture Flux, a domain specific language to build data flows for metadata processing.

# Installing Flux

There are two options: either you check out the source code in your favorite IDE, or you unzip a distribution of Metafacture into a directory of your choice. A distribution can be downloaded here: http://culturegraph.github.com

You might also want to take a look at the Metafacture-IDE project: [[https://github.com/culturegraph/metafacture-ide]].

# Running Flux

If you are working with the source code directly, execute the class `org.culturegraph.mf.Flux`. If you are working with a distribution of Metafacture, execute the script `flux.sh` or `flux.bat` in the `bin/` folder.

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
The following snippet shows a simple flux file:
```c
//declare variables
default file = FLUX_DIR + "10.marc21";

//declare flow
file|
open-file|
as-lines|
decode-marc21|
morph(FLUX_DIR + "morph-marc21.xml")|
encode("literals")|
write("stdout");
```
In the first section variables are declared, in the second, we define the flow.
Linebreaks are optional. Semicolons `;` mark the end of a variable assignment or flow definition. 

##Variables
Variables are always Strings and can be concatenated with the `+` operator. Escape sequences follow the Java String conventions: `\n`=line break, `\t`=tab, `\\`=\, `\u0024`=unicode character, etc.

The `default` keyword tells Flux to assign the respective value _only_ if the variable has 
not yet been set on the command line. Without `default`, previous assignments will be overwritten.

Paths are always relative to the directory within which the flux command is executed. To address files relative to the location of the executed flux file, use the predefined `FLUX_DIR` variable.

##Comments
Flux supports single line C/Java-style comments: `//comment`.

##Flow Definitions

The syntax for defining flows takes its cues from bash pipes. Commands are concatenated with the pipe character `|`. 

Some commands take a constructor argument. It is provided within brackets: `command("arg")`.
Furthermore, some commands have named options. These are set as follows `command(optionname="arg1",annotheroption="arg2")` or with constructor argument: `command("arg",option="arg2")`.
To learn about the available options of a command, execute Flux without commands: It will list all available commands, including options.

To some commands, the entire environment can be given as argument. This is done with the `*` character: `morph("morphdef.xml", *)`. In this case Metamorph gains access to all variable assignments made in Flux.
(See also [[Metamorph-Definition-Language#parameters-to-metamorph-definitions]]).

Note that unlike shell pipes, the data flowing between Flux commands is _typed_. This means that only commands with matching signatures can be combined. To lookup the signatures, execute Flux without arguments. It will list all available commands, including signatures.

## Getting Help and Inspiration
1. If executed without arguments, Flux will display a short help text along with a list of all registered commands. 
2. There are several example flux files along with sample data in the folder `examples/`: https://github.com/culturegraph/metafacture-core/tree/master/examples

# Adding new Commands

_under construction_