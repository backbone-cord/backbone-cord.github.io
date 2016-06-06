binding
-------------------------------

Binds element attributes and text-based children to observable keys contained within `{observerableKey}` formatters.

```javascript
h('div', {data-name: '{name}'}, 'Example with name bound here: {name}.');
```

#### Special Binding Attributes

The following are recognized as attributes but do not go through the `setAttribute()` function instead they are set directly on the element.

* value - also invokes a change DOM event
* checked - also invokes a change DOM event
* innerHTML

#### Reverse Binding

Reverse binding can be done with special `input` or `change` attributes, each corresponding to those triggered DOM events. The value must be an observable key, where {} formatters aren't used

```javascript
h('input', {change:'key'});
```

#### Reverse Binding Mapping

When reverse binding changes for the following they are mapped using the corresponding code to types other than strings.

* `range` - `parseInt(el.value)`
* `number` - `Number(el.value)`
* `integer` - `parseInt(el.value)`
* `decimal` - `parseFloat(el.value)`
* `date` - `new Date(el.value)`
* `datetime` - `new Date(el.value)`
* `checkbox` - `el.checked`

#### Restrictions

* Any DOM attribute can be bound to a variable or a formatted string, however it is recommended to NOT use the style attribute in this way (as a formatted string) because any styles applied with javascript, like with the hidden plugin will get lost when the style attribute is updated as a string on an observer callback.
* Circular binding values is currently not supported, where the `value` is the attribute is set to the same key as `change` because when value changes this plugin will invoke a `change` event. Reverse binding on `input` is ok.
