interpolation
-------------------------------

The interpolation plugin simply adds another observer method to the view which can observe many keys at once and invoke the observer with a single formatted string.

`observeFormat(format, observer, immediate)`

#### Example

```javascript
view.observeFormat('Key 1 is equal to {key1} and key 2 is equal to {key2}', this.obsFunction)
```
