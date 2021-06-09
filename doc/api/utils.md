## Class: `mime.MIMEType`

<!-- YAML
added: REPLACEME
-->

> Stability: 1 - Experimental

An implementation of [the MIMEType class](https://bmeck.github.io/node-proposal-mime-api/).

In accordance with browser conventions, all properties of `MIMEType` objects
are implemented as getters and setters on the class prototype, rather than as
data properties on the object itself.

A MIME string is a structured string containing multiple meaningful
components. When parsed, a `MIMEType` object is returned containing
properties for each of these components.

### Constructor: `new MIMEType(input)`

* `input` {string} The input MIME to parse

Creates a new `MIMEType` object by parsing the `input`.

```js
new MIMEType('text/plain');
```

A `TypeError` will be thrown if the `input` is not a valid MIME. Note
that an effort will be made to coerce the given values into strings. For
instance:

```js
const myMIME = new MIMEType({ toString: () => 'text/plain' });
console.log(String(myMIME));
// Prints: text/plain
```

#### `mime.type`

* {string}

Gets and sets the type portion of the MIME.

```js
const myMIME = new MIMEType('text/javascript');
console.log(myMIME.type);
// Prints: text

myMIME.type = 'application';
console.log(myMIME.type);
// Prints: application
```

#### `mime.subtype`

* {string}

Gets and sets the subtype portion of the MIME.

```js
const myMIME = new MIMEType('text/ecmascript');
console.log(myMIME.subtype);
// Prints: ecmascript

myMIME.subtype = 'javascript';
console.log(myMIME.subtype);
// Prints: javascript
```

#### `mime.essence`

* {string}

Gets the essence of the MIME. This property is read only.
Use `mime.type` or `mime.subtype` to alter the MIME.

```js
const myMIME = new MIMEType('text/javascript');
console.log(myMIME.essence);
// Prints: text/javascript

myMIME.type = 'application';
console.log(myMIME.essence);
// Prints: application/javascript
```

#### `mime.params`

* {MIMEParams}

Gets the [`MIMEParams`][mimeparams] object representing the
parameters of the MIME. This property is read-only. See
[`MIMEParams`][mimeparams] documentation for details.

#### `mime.toString()`

* Returns: {string}

The `toString()` method on the `MIMEType` object returns the serialized MIME.
The value returned is equivalent to that of [`mime.toJSON()`][mime.toJSON].

Because of the need for standard compliance, this method does not allow users
to customize the serialization process of the MIME.

#### `mime.toJSON()`

* Returns: {string}

The `toJSON()` method on the `MIMEType` object returns the serialized MIME. The
value returned is equivalent to that of [`mime.toString()`][mime.toString].

This method is automatically called when an `MIMEType` object is serialized
with [`JSON.stringify()`][].

```js
const myMIMES = [
  new MIMEType('img/png'),
  new MIMEType('img/gif'),
];
console.log(JSON.stringify(myMIMES));
// Prints: ["img/png", "img/gif"]
```

### Class: `util.MIMEParams`
<!-- YAML
added: REPLACEME
-->

The `MIMEParams` API provides read and write access to the parameters of a
`MIMEType`.

#### Constructor: `new MIMEParams()`

Creates a new `MIMEParams` object by with empty parameters

```js
new MIMEParams();
```

#### `mimeParams.delete(name)`

* `name` {string}

Remove all name-value pairs whose name is `name`.

#### `mimeParams.entries()`

* Returns: {Iterator}

Returns an ES6 Iterator over each of the name-value pairs in the parameters.
Each item of the iterator is a JavaScript Array. The first item of the Array
is the `name`, the second item of the Array is the `value`.

Alias for [`mimeParams[@@iterator]()`][mimeparams_iterator].

#### `mimeParams.get(name)`

* `name` {string}
* Returns: {string} or `null` if there is no name-value pair with the given
  `name`.

Returns the value of the first name-value pair whose name is `name`. If there
are no such pairs, `null` is returned.

#### `mimeParams.has(name)`

* `name` {string}
* Returns: {boolean}

Returns `true` if there is at least one name-value pair whose name is `name`.

#### `mimeParams.keys()`

* Returns: {Iterator}

Returns an ES6 Iterator over the names of each name-value pair.

```js
const { params } = new MIMEType('text/plain;foo=0;bar=1');
for (const name of params.keys()) {
  console.log(name);
}
// Prints:
//   foo
//   bar
```

#### `mimeParams.set(name, value)`

* `name` {string}
* `value` {string}

Sets the value in the `MIMEParams` object associated with `name` to
`value`. If there are any pre-existing name-value pairs whose names are `name`,
set the first such pair's value to `value`.

```js
const { params } = new MIMEType('text/plain;foo=0;bar=1');
params.set('foo', 'def');
params.set('baz', 'xyz');
console.log(params.toString());
// Prints: foo=def&bar=1&baz=xyz
```

#### `mimeParams.values()`

* Returns: {Iterator}

Returns an ES6 Iterator over the values of each name-value pair.

#### `mimeParams\[@@iterator\]()`

* Returns: {Iterator}

Returns an ES6 Iterator over each of the name-value pairs in the query string.
Each item of the iterator is a JavaScript Array. The first item of the Array
is the `name`, the second item of the Array is the `value`.

Alias for [`mimeParams.entries()`][mimeparams.entries].

```js
const { params } = new MIMEType('text/plain;foo=bar;xyz=baz');
for (const [name, value] of params) {
  console.log(name, value);
}
// Prints:
//   foo bar
//   xyz baz
```

[mime.toJSON]: #mime_mime_tojson
[mime.toString]: #mime_mime_tostring
[mimeparams_iterator]: #mime_mimeparams_iterator
[mimeParams.entries]: #mime_mimeparams_entries
[mimeparams]: #mime_class_util_mimeparams
