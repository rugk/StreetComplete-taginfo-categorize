## Prerequisites
- make, perl, JSON perl module, json_reformat (on Debian-based system: `apt-get install make perl libjson-perl yajl-tools`)
- up-to-date `https://github.com/openstreetmap/id-tagging-schema` in `../id-tagging-schema`

## Usage instructions

Those scripts generate `KEYS_THAT_SHOULD_BE_REMOVED_WHEN_SHOP_IS_REPLACED` for the [StreetComplete](https://github.com/streetcomplete/StreetComplete) project.

`make update` should be run periodically, to find new keys with usage > 0.01%
and add them to `## TODO ##` section at the bottom of `keys.txt` file.

User should then investigate and move them from there to correct section
like `### KEYS TO REMOVE ###` or `### KEYS TO KEEP ###` sections of `keys.txt`.

Running `make` again will then generate `sc_to_remove.txt` with kotlin code to copy/paste to
https://github.com/streetcomplete/StreetComplete/blob/master/app/src/main/java/de/westnordost/streetcomplete/data/meta/OsmTaggings.kt
It is generated only from data in `### KEYS TO REMOVE ###` section.

## Notes on `keys.txt` format:

* each line can contain a regex (or normal text) representing key (in-line comments are possible too, as everything after `//` or `#` is ignored
* blank lines in `### KEYS TO REMOVE ###` section will become newlines in `sc_to_remove.txt`
* lines beginning with `//` in `### KEYS TO REMOVE ###` section will be copied to `sc_to_remove.txt`
* lines beginning with `#` are ignored completely

## when isShopExpressionFragment() changes

If `isShopExpressionFragment()` in `OsmTaggings.kt` changes, then
`FETCH_KEYS` / `FETCH_TAGS` in `Makefile` should be updated too, and
scripts re-run (i.e. `make update` + `make`) in order to generate new
`KEYS_THAT_SHOULD_BE_REMOVED_WHEN_SHOP_IS_REPLACED` to put in
`OsmTaggings.kt`.
