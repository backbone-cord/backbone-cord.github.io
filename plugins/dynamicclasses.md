dynamicclasses
-------------------------------

Adds support for CSS class names that may be changed dyanmically using the value of an observable key. The following example sets the class of a button to the value of key with the myclass- prefix, e.g. `myclass-red` or `myclass-blue`.

#### Example

```javascript
h('button.myclass-{key}');
```
#### Technical Details

The plugin will also add a psuedo class name with the format `dynamic-class-{random()}` not for any styling purposes but to locate act as a bookmark to locate and change the dynamic class name.
