# Example 1: Organising tokens into partials

One use-case for `$includes` is to divide a large set of design tokens into smaller files for easier maintenance.

In this example, `colors.tokens.json`, `spacing.tokens.json` and `typography.tokens.json` are our "partial" files, each containing various design tokens.

The `index.tokens.json` file then includes all three, thus giving any tool that loads `index.tokens.json` access to the combined set of tokens from all three partials.
