We plan to overhaul the syntax of Flux. Suggestions are welcome and collected here.

# Suggestion from Christoph

```
file-reference := '<' file-literal '>'

route-expression := router-module '<' route-list

router-module := module

route-list := route (',' route)*

route := (flow-reference | variable-reference) '[' condition ']'

condition := parameter-list

flow-reference := '@' flow-name

flow-name := identifier

module-name := identifier

identifier := [a-zA-Z_] [a-zA-Z0-9_-]*

file-literal := ... any valid file name

string-literal := '"' ... '"'

integer-literal := ('-'|'+')? [0-9]+

float-literal := ...

boolean-literal := 'true' | 'false'
```

Variable references can be used to make parts of a flow configurable:

```
read-xml: open-file | decode-xml | $handle-xml
process-wiki-dump: @read-xml(handle-xml=handle-wiki-xml) | analyze-wp($analyzers)

main: "wiki-dump.xml" | @process-wiki-dump(analyzers="analyzers.conf") | pretty-print | stdout
```

Variables can reference flows as  well as values.

File references are automatically expanded. If the file-reference is not at the value position of a parameter it is treated as a properties file whose properties are used as the parameters. The user may provide additional parameters after the file-references which overwrite the parameters in the file. Parameters before the file reference may be overwriten by parameters from the file.

If the file-reference is used instead of a value then the file is loaded as a text file and its contents are used as the paramter value.