# ORA-Tables
This project contains data for lookup tables used by [ORA](https://www.ora-externsion.com), the Online Repository Assistant.

If you would like to suggest modifications to these tables, please submit a pull request. If you are not familiar with how GitHub works, you can create an issue (see the link above) and describe the addition or change in sufficient detail that I can make the change for you.

## Table Formats

The tables are stored here in [JSON](https://en.wikipedia.org/wiki/JSON) format and are in the JSON subfolder. Most files are locale-specific and the filenames begin with a two-character code that indicates the locale, `us` for United States, `ca` for Canada, etc.

The tables include a row for each item with at least two text strings, a *key* and a *value*. The key is often an abbreviation, such as a state abbreviation. The exact format varies according to the table types which are described below.

Some keys are paired with more than one value. For example, the us_states.json file has an entry:

````json
["MA", "Massachusetts", "Mass"]
````

That defines "MA" as an abbreviation for "Massachusetts", and the entry can be found by either the key or the value. The entry also includes "Mass", which is an *alias*. In effect, "Mass" is another text value that can be used to find the standard abbreviation (MA) and standard full name (Massachusetts). 

The tables include metadata that includes an `id` and a `type`. The types are:

- **Associative** tables associate a key with a value. Associative tables can be used with ORA's [`lookup`](https://www.ora-extension.com/en/transforms.htm#transform-lookup) transform to convert the key to the value.

- **Bi-directional** tables allow an entry to be found via the key, the value, and zero or more alias values. Bi-directional tables can be used with ORA's [`abbr`](https://www.ora-extension.com/en/transforms.htm#transform-abbr) and [`full`](https://www.ora-extension.com/en/transforms.htm#transform-full) transforms. `abbr` will return the key and `full` will return the value. So, for example, if the State field has a value "MA", "Massachusetts", or "Mass", `[State:abbr:us_states]` will return "MA" whereas `[State:full:us_states]` will return "Massachusetts".

- **Custom** tables have formats that are unique to their `id`. For example, the us_counties.json table has one entry for each US state and the entry is an array of county names. This table cannot be used directly by any ORA transform. It is used behind the scenes by ORA's [Place Transforms](https://www.ora-extension.com/en/transforms-place.htm).
