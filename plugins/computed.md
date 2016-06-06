computed
-------------------------------

The computer plugin enables declared properties to automatically recompute when dependent values change.

#### Usage

To use, define a property with a get function and include observable keys are arguments. When called, use those arguments directly to compute the value.

#### Example

```javascript
properties: {
	c: {
		get: function(a, b) {
			return Math.sqrt(a * a + b * b);
		}
	}
}
```

#### Computed Model Attributes

Also included with the plugin is the `_addComputed(key, func)` function which can be used to add computed attributes to a model instance. The same argument rules apply to the function but the observable arguments can only be other attributes on the model. To access a computed attribute on a model use the `get(key)` function.
