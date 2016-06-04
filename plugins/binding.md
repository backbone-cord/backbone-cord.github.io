#### binding

Binds element attributes and text-based children to variables.

```javascript
h('div', {data-name: '{name}'}, 'Example with name bound here: {name}.');
```

*NOTE:* Any DOM attribute can be bound to a variable or a formatted string, however it is recommended to NOT use the style attribute in this way because any styles applied with javascript, like with the hidden plugin will get lost when the style attribute is set on a value getting updated.

#### binding (reverse)

Reverse binding can be done with special `input` or `change` attributes, each corresponding to those triggered events.

```javascript
h('input', {change:'varName'});
```
