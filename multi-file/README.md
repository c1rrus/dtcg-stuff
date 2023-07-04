# `$include` proposal for working with multiple files

## Idea

Add a new `$include` property for the root group, whose value is an array of relative file paths to other DTCG files. Those files are merged into the current file in the order they were listed.

Normally, tokens in a DTCG file can only reference other tokens within the same file. However, tokens in a file that `$includes` other files can reference tokens from any of the included files too.

### Examples

1. [Organising tokens into partials](./example01-partials/)