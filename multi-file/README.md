# `$includes` proposal for working with multiple files

## Idea

Add a new `$includes` property for the root group, whose value is an array of relative file paths to other DTCG files. The design tokens in the included files are merged together and then the tokens from the including file are merged in.

```json
{
  "$includes": [
    "path/to/file-a.tokens.json",
    "path/to/file-b.tokens.json",
    "path/to/file-c.tokens.json"
  ]
}
```

If a design token with the same path occurs in several included files, then the one from the latest file in the `$includes` array replaces the ones from any earlier files. Finally, if a token with the same path exists in the including file, then that replaces the one(s) from the included file(s).

Normally, tokens in a DTCG file can only reference other tokens within the same file. However, tokens in a file that `$includes` other files can reference tokens from any of the included files too. When resolving token values, the dereferencing must happen _after_ all included tokens have been merged.

```jsonc
// base.tokens.json
{
    "white": {
        "$value": "#ffffff",
        "$type": "color"
    },
    "black": {
        "$value": "#000000",
        "$type": "color"
    }
}
```

```jsonc
// light-theme.tokens.json
{
    "$includes": [ "base.tokens.json" ],
    "foreground": {
        "$type": "color",
        "$value": "{black}" // works because base.tokens.json was included
    },
    "background": {
        "$type": "color",
        "$value": "{white}"
    },
    "button-text-color": {
        "$type": "color",
        "$value": "{foreground}" // -> {black} -> #000000
    },
    "button-bg-color": {
        "$type": "color",
        "$value": "{background}" // -> {white} -> #ffffff
    }
}
```

```jsonc
// dark-theme.tokens.json
{
    // "patches" the light-theme, to invert foreground & background
    "$includes": [ "light-theme.tokens.json" ],
    "foreground": {
        "$type": "color",
        "$value": "{white}" // works because base.tokens.json was included
    },
    "background": {
        "$type": "color",
        "$value": "{black}"
    }

    // button-text-color is available as it was included,
    // but now resolves as: -> {foreground} -> {white} -> #ffffff

    // button-bg-color is available as it was included,
    // but now resolves as: -> {background} -> {white} -> #000000
}
```

### Examples

1. [Organising tokens into partials](./example01-partials/)
