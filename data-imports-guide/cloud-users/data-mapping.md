---
title: Data mapping and manipulation
layout: article
keywords: handle, update imported users
---
# Data mapping and manipulation

When you create your import configuration, a large part of what you are doing is mapping data. You are mapping your user data from your source (such as from Entra ID or Google Workspace) into target Hornbill user fields. You complete most of this mapping by typing in fields and by selecting from dropdowns in the Cloud Data Imports user interface. It is important to note, however, that some basic [string manipulation methods](/data-imports-guide/cloud-users/data-mapping#applying-transforms) are also available.

## Mapping data with tags

The mapping of source data into user fields in Hornbill Cloud Data Imports is done using mustache tags. Tags are wrapped in double curly braces. For example `{{givenName}}` is a tag; another one is `{{#givenName}}`. In both examples, `givenName` is referred to as the *key* or *tag key*. See the [Mustache specification](https://mustache.github.io/mustache.5.html) for detailed information about supported tag types.

You can use multiple tags in the same mapping configuration field, should you wish to use more than one field from the source data record.

For example, when importing user records from your source, imagine your records each include a first name and a last name, but no handle. A *handle* --- also known as a *display name* --- is needed in Hornbill. The handle captures both the first name and the last name in one field. Because your source records don't include a handle, you have to create one. In the Data Source tab, in the User Properties dialog, you specify that the handle is `{{givenName}} {{familyName}}`. As shown below, you're populating the Handle field in Hornbill with the user's given name, followed by a space, followed by the family name.

![Creating a handle](/_books/data-imports-guide/cloud-users/images/givenname-plus-surname-makes-handle.png)

If you're performing updates against your imported users, you can empty the value of any supported text column using the following notation:

> `__clear__`

### Using auto-complete

When populating the user property fields, where relevant, and once you start typing a field name, you will be presented with a list of matching fields to choose from. These are the default output field sets for the platform you are importing user records from. 

To auto-complete the field, choose the field you want, then select it using a mouse click or using the Tab or Enter key on your keyboard.

![Mapping Auto-Complete](/_books/data-imports-guide/cloud-users/images/cloud-import-autocomplete.png)

## Applying transforms

<!--
A transform will use the fields but in a much more complicated way. It’s just a complicated mapping.
-->

When mapping user record fields from your source system into Hornbill user fields, there may be situations where the data you receive is not in the exact format you want to store, display, or work with in Hornbill. By applying transformations in your mappings, you can change data from one thing to another during the import. This is useful to ensure consistency, accuracy, and usability of your user records.

For example, in a custom attribute (Attrib 7) in the image below, the admin has added the `replace` transform to the mapping so as to change the department name *Research* to *R&D* when the user records are imported.

![Using the Replace transform](/_books/data-imports-guide/cloud-users/images/transform-replace.png)

Consider the following uses for data transforms:

* **Formatting data** – Standardizing phone numbers, capitalizing names, or converting usernames into a preferred format.
* **Cleaning up values** – Removing unnecessary characters, trimming spaces, or handling special characters.
* **Deriving new values** – Constructing email addresses, combining first and last names, or generating display names.
* **Ensuring compatibility** – Converting values to match Hornbill’s required field types or structures.
* **Business-specific adjustments** – Applying company naming conventions, appending department codes, or localizing job titles.

You can [use multiple transforms in the same mapping configuration field](/data-imports-guide/cloud-users/data-mapping#using-multiple-transforms).

The following is a list of the supported transformations, along with examples of how they can be used.

### lowerCase

Performs a lowercase operation on the string (or tag output) within it. In the below example, the value of `givenName` from the source data record would be converted to lower case and applied to the field it is being mapped into:

> `{{givenName | lowerCase}}`

**Arguments:**

* *(none)* – only the source string is required.

### upperCase

Performs an uppercase operation on the string (or tag output) within it. In the below example, the value of `givenName` and `familyName`, concatenated with a space between them, from the source data record would be converted to upper case and applied to the field it is being mapped into:

> `{{givenName | upperCase}} {{familyName  | upperCase}}`

**Arguments:**

* *(none)* – only the source string is required.

### trim

Performs a trim operation on the string (or tag output) within it. In the below example, the value of `givenName` from the source data record would have any prefix and postfix spaces removed before the string is applied to the field it is being mapped into:

> `{{givenName | trim}}`

**Arguments:**

* *(none)* – only the source string is required.

### replace

Performs a first-hit string replace on the source data record field, using JSON config to pass through the source string, the string to be replaced, and the string to replace it with. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `your.user@hotmail.com` being applied to the field it is being mapped into:  

> `{{email | replace : '@hornbill.com' : '@hotmail.com'}}`

**Arguments:**

- The first argument is the string to replace (if found), `hornbill.com` in this example.
- The second argument is the string to replace the found string with , `@hotmail.com` in this example.

### replaceAll

Performs an all-hit string replace on the source data record field, using JSON config to pass through the source string, the string to be replaced, and the string to replace it with. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `yaur.user@harnbill.cam` being applied to the field it is being mapped into:  

> `{{email | replaceAll : 'o' : 'a'}}`

**Arguments:**

- The first argument is the string to replace all/any instance of (if found), `o` in this example.
- The second argument is the string to replace the found string(s) with , `a` in this example.

### regexMatch

Performs a regular expression against the source data record field, and returns the matching string if one is found. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `hornbill` being applied to the field it is being mapped into:  

> `{{email | regexMatch : "(HORNBILL)" : "gi"}}`

**Arguments:**

- The first argument is the regular expression to run against the source, `(HORNBILL)` in this example.
- The second argument is a list of switches to apply to the regular expression, `gi` in this example.

### regexReplace

Performs a regular expression against the source data record field, and replaces the matching string with the provided replacement if one is found. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `your.user@hotmail.com` being applied to the field it is being mapped into:  

> `{{email | regexReplace : '(HORNBILL)' : 'hotmail'  : 'gi'}}`

**Arguments:**

- The first argument is the regular expression to run against the source, `(HORNBILL)` in this example.
- The second argument is the string to replace the matched substring with, `hotmail` in this example.
- The third argument is a list of switches to apply to the regular expression, `gi` in this example.

### padStart

Pads the start of the string in the source data record field with zero or more characters, to a provided string-total maximum length. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `***your.user@hornbill.com` being applied to the field it is being mapped into:  

> `{{email | padStart : 25 : '*'}}`

**Arguments:**

- The first argument is the maximum length of the returned string, `25` in this example.
- The second argument is the string to pad the start of the source string with, `*` in this example.

### padEnd

Pads the end of the string in the source data record field with zero or more characters, to a provided string-total maximum length. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `your.user@hornbill.com***` being applied to the field it is being mapped into:  

> `{{email | padEnd : 25 : '*'}}`

**Arguments:**

- The first argument is the maximum length of the returned string, `25` in this example.
- The second argument is the string to pad the end of the source string with, `*` in this example.

### slice

Returns the part of the string from the start index up to and excluding the end index, or to the end of the string if no end index is supplied. Slice supports negative indexing to return X number of characters from the end of the string. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `rnbil` being applied to the field it is being mapped into:  

> `{{email | slice : -10 : -5}}`

**Arguments:**

- The first argument is the start index for the slice.
- The second argument is the end index for the slice.

### subString

Returns the part of the string from the start index up to and excluding the end index, or to the end of the string if no end index is supplied. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `user@` being applied to the field it is being mapped into:  

> `{{email | subString : 5 : 10}}`

**Arguments:**

- The first argument is the start index for the substring.
- The second argument is the end index for the substring.

### countryCode

Converts a country name into its corresponding ISO2 country code (two-letter abbreviation). If the provided country name is not recognized, an empty string will be returned.

For example, if the source field `country` contained `United Kingdom`, this would result in `GB`:

> `{{country | countryCode}}`

**Arguments:**

* *(none)* – only the source string (country name) is required.

### wrap

Wraps the string with a prefix (`left`) and/or suffix (`right`). This is useful for ensuring certain characters or formatting are always present around a value.

For example, if the source field `username` contained `j.smith`, this would result in `[j.smith]`:

> `{{username | wrap : "[" : "]"}}`

**Arguments:**

* The first argument is the string to prepend (left).
* The second argument is the string to append (right).

### lookup

Looks up a value from a provided dictionary (in JSON format). If the string exists as a key in the dictionary, the corresponding value will be returned. If not found, an empty string is returned.

For example, if the source field `departmentCode` contained `IT`, and you provided a lookup dictionary of department codes to full names, the result would be `Information Technology`:

> `{{departmentCode | lookup : '{"HR":"Human Resources","IT":"Information Technology"}'}}`

**Arguments:**

* A JSON object where the keys are possible input values and the values are the desired outputs.

### add

Adds a numeric value to the source number. If the source value or argument cannot be converted to a number, the result is an empty string.

For example, if the source field `employeeId` contained `100`, this would result in `110`:

> `{{employeeId | add : 10}}`

**Arguments:**

* The number to add to the source value.

### subtract

Subtracts a numeric value from the source number. If the source value or argument cannot be converted to a number, the result is an empty string.

For example, if the source field `leaveBalance` contained `25`, this would result in `20`:

> `{{leaveBalance | subtract : 5}}`

**Arguments:**

* The number to subtract from the source value.

### multiply

Multiplies the source number by a given value. If the source value or argument cannot be converted to a number, the result is an empty string.

For example, if the source field `workingHours` contained `7.5`, this would result in `37.5`:

> `{{workingHours | multiply : 5}}`

**Arguments:**

* The number to multiply the source value by.

### divide

Divides the source number by a given value. If the source value or argument cannot be converted to a number, or if division by zero is attempted, the result is an empty string.

For example, if the source field `totalStorage` contained `1024`, this would result in `1`:

> `{{totalStorage | divide : 1024}}`

**Arguments:**

* The number to divide the source value by.

## Using multiple transforms

You can use multiple transforms in the same mapping configuration field, and they will be applied in the specified order. In the below example, if the source data record field `email` contained `your.user@hornbill.com`, this would result in `HORNBILL.COM` being applied to the field it is being mapped into. This is because we're applying a `slice` transform to remove `your.user@`, then applying an `upperCase` transform on the rest of the string.

> `{{email | slice : -12 | upperCase }}`