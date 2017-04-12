binding
-------------------------------

Binds element attributes and text nodes to observable keys contained within interpolated `[observerableKey]` keys.

#### Example

Example showing data-name attribute bound to `this.name` and input value bound to `this.age`.

```javascript
h('div', {data-name: '[name]'},
	'Example with name bound here: [name].',
	h('input', {'data-type': 'int' bind: 'age', placeholder: 'Enter your age'})
);
// or in JSX
<div data-name="[name]">
	Example with name bound here: [name].
	<input data-type="int" bind="age" placeholder="Enter your age"></input>
</div>;
```

#### Special Binding Attributes

The following are recognized as attributes but do not go through the `setAttribute()` function instead they are set directly on the element and the do NOT accept `[observerableKey]` syntax.

* `bind` - shortcut for setting both observe and change to the same key
* `observe` - the value of this element encoded with the observable value when it changes
* `change` - the value of this element is decoded to the key when the user changes it
* `input` - the value of this element is decoded to the key when the user enters any input
* `innerHTML` - the value of this element's innerHTML is set when the observable value changes

#### Value Encoding/Decoding

Value encoding and decoding functions are specified in a single object under `Backbone.Cord.encoders` and `Backbone.Cord.decoders`. Both encoding and decoding functions are called based on the given `type` or `data-type` attribute of the element.

The following decoders are built-in:

* `range`
* `number`
* `int`
* `integer`
* `float`
* `decimal`
* `date`
* `datetime`
* `bool`
* `checkbox`

The following encoders are built-in:

* `date`
* `datetime`
* `bool`
* `checkbox`

**NOTE:** If a data-null attribute is found on the element than any empty value will be given as `null` instead of an empty string.
