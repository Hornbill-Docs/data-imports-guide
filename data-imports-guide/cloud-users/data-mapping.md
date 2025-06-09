# Data Mapping and Manipulation

Hornbill provides the ability for data import administrators to map their source field data into target Hornbill User fields. Some basic string manipulation methods are also available.

## Mapping with Tags

The mapping of source data into User fields in Hornbill Cloud Data Imports is done using mustache tags. Tags are wrapped in double curly braces. For example `{{givenName}}` is a tag; another one is `{{#givenName}}`. In both examples, we'd refer to `givenName` as the key or tag key. See the [Mustache specification](https://mustache.github.io/mustache.5.html) for detailed information about supported tag types.

You can use multiple tags in the same mapping configuration field, should you wish to use more than one field from the source data record. Populating the Display Name field in Hornbill with the user's given name, followed by a space, followed by the family name, is one example where this may be useful:

> `{{givenName}} {{familyName}}`

If you're performing updates against your imported users, you can empty the value of any supported text column using the following notation:

> `__clear__`

### Mapping Auto-Complete

When populating the User Property fields, where relevant, and once you start typing a field name, you will be presented with a list of matching fields to choose from. These are the default output field sets for the platform you are importing user records from. Choosing the field you want, and selecting with either a mouse click, or by pressing the Tab or Enter key on your keyboard, will auto-complete the field for you.

![Mapping Auto-Complete](/_books/data-imports-guide/cloud-users/images/cloud-import-autocomplete.png)

## Transforms

The following is a list of supported transforms, and examples of use.

### lowerCase

Performs a lowercase operation on the string (or tag output) within it. In the below example, the value of `givenName` from the source data record would be converted to lower case and applied to the field it is being mapped into:

> `{{#lowerCase}}{{givenName}}{{/lowerCase}}`

### upperCase

Performs an uppercase operation on the string (or tag output) within it. In the below example, the value of `givenName` and `familyName`, concatenated with a space between them, from the source data record would be converted to upper case and applied to the field it is being mapped into:

> `{{#upperCase}}{{givenName}} {{familyName}}{{/upperCase}}`

### trim

Performs a trim operation on the string (or tag output) within it. In the below example, the value of `givenName` from the source data record would have any prefix and postfix spaces removed before the string is applied to the field it is being mapped into:

> `{{#trim}} {{givenName}} {{/trim}}`

### replace

Performs a first-hit string replace on the source data record field, using JSON config to pass through the source string, the string to be replaced, and the string to replace it with. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `your.user@hotmail.com` being applied to the field it is being mapped into:  

```json
{{#replace}}{"source":"{{email}}", "replace": "@hornbill.com", "with":"@hotmail.com"}{{/replace}}

```

The `replace` operation expects a JSON object to be presented to it, containing three properties:

- `source` - The string to perform the operation on
- `replace` - The string to be replaced, if it exists in the `source` string
- `with` - The string to replace the `replace` string with, from the `source` string 

### replaceAll

Performs an all-hit string replace on the source data record field, using JSON config to pass through the source string, the string to be replaced, and the string to replace it with. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `yaur.user@harnbill.cam` being applied to the field it is being mapped into:  

```json
{{#replaceAll}}{"source":"{{email}}", "replace": "o", "with":"a"}{{/replaceAll}}

```

The `replaceAll` operation expects a JSON object to be presented to it, containing three properties:

- `source` - The string to perform the operation on
- `replace` - The string to be replaced, if it exists in the `source` string
- `with` - The string to replace the `replace` string with, from the `source` string 

### regexMatch

Performs a regular expression against the source data record field, and returns the matching string if one is found. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `hornbill` being applied to the field it is being mapped into:  

```json
{{#regexMatch}}{"source": "{{email}}", "expression": "(HORNBILL)", "switches": "gi"}{{/regexMatch}}

```

The `regexMatch` operation expects a JSON object to be presented to it, containing three properties:

- `source` - The string to perform the operation on
- `expression` - The regular expression to run against the source
- `switches` - The switches to apply to the regular expression

### regexReplace

Performs a regular expression against the source data record field, and replaces the matching string with the provided replacement if one is found. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `your.user@hotmail.com` being applied to the field it is being mapped into:  

```json
{{#regexReplace}}{"source": "{{email}}", "expression": "(HORNBILL)", "with": "hotmail","switches": "gi"}{{/regexReplace}}

```

The `regexReplace` operation expects a JSON object to be presented to it, containing four properties:

- `source` - The string to perform the operation on
- `expression` - The regular expression to run against the source
- `with` - The string to replace the matched substring with
- `switches` - The switches to apply to the regular expression

### padStart

Pads the start string in the source data record field with zero or more characters, to a provided string-total maximum length. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `***your.user@hornbill.com` being applied to the field it is being mapped into:  

```json
{{#padStart}}{"source": "{{email}}", "targetLength": 25, "padString": "*"}{{/padStart}}

```

The `padStart` operation expects a JSON object to be presented to it, containing three properties:

- `source` - The string to perform the operation on
- `targetLength` - The maximum length of the returned string
- `padString` - The string to pad the start of the source string with

### padEnd

Pads the end of the string in the source data record field with zero or more characters, to a provided string-total maximum length. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `your.user@hornbill.com***` being applied to the field it is being mapped into:  

```json
{{#padEnd}}{"source": "{{email}}", "targetLength": 25, "padString": "*"}{{/padEnd}}

```

The `padEnd` operation expects a JSON object to be presented to it, containing three properties:

- `source` - The string to perform the operation on
- `targetLength` - The maximum length of the returned string
- `padString` - The string to pad the end of the source string with

### slice

Returns the part of this string from the start index up to and excluding the end index, or to the end of the string if no end index is supplied. Slice supports negative indexing to return X number of characters from the end of the string. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `rnbil` being applied to the field it is being mapped into:  

```json
{{#slice}}{"source":"{{email}}", "from": -10, "to": -5}{{/slice}}

```

The `slice` operation expects a JSON object to be presented to it, containing three properties:

- `source` - The string to perform the operation on
- `from` - The start index for the slice
- `to` - The end index for the slice

### substring

Returns the part of this string from the start index up to and excluding the end index, or to the end of the string if no end index is supplied. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `user@` being applied to the field it is being mapped into:  

```json
{{#substring}}{"source":"{{email}}", "from": 5, "to": 10}{{/substring}}

```

The `substring` operation expects a JSON object to be presented to it, containing three properties:

- `source` - The string to perform the operation on
- `from` - The start index for the substring
- `to` - The end index for the substring


## Using Multiple Transforms

You can use multiple transforms in the same mapping configuration field, and they will be applied in an inside-out order. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `HORNBILL.COM` being applied to the field it is being mapped into. This is because we're applying a `slice` transform to remove `your.user@`, then applying an `upperCase` transform on the rest of the string.

```json
{{#upperCase}}{{#slice}}{"source":"{{email}}", "from": -12}{{/slice}}{{/upperCase}}

```