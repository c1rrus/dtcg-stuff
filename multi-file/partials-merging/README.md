Instead of adding a `$type` property to each token in `colors.tokens.json` and `sizes.tokens.json`, it's been hoisted up to the root group. The _resolved_ type of the tokens in each file is unchanged.

However, different approaches to merging the two into `index.tokens.json` will yield different results:

## Deep-merge of JSON data

Simply merging the raw JSOn data would yield a result like:

```json
{
    "$type": "dimension",
    "color": {
        "red": {
            "$value": "#ff0000"
        },
        "green": {
            "$value": "#00ff00"
        },
        "blue": {
            "$value": "#0000ff"
        }
    },
    "size": {
        "small": {
            "$value": "0.25rem"
        },
        "medium": {
            "$value": "0.5rem"
        },
        "large": {
            "$value": "1rem"
        }
    }
}
```

...because the `$type` of the root group  `sizes.tokens.json` replaces the one from `colors.tokens.json`.

However, the resulting data is no longer valid DTCG data since the 3 tokens in the `color` group now have a resolved type of `dimension`, but their `$value`s are not valid dimension values.

## Resolving types and then 