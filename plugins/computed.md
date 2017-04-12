computed
-------------------------------

The computed plugin enables view properties to automatically recompute when dependent values change.

#### Usage

To use, define a property with a get function and include the oservable keys are arguments. The observable keys however must be specified with __ instead of . syntax because . is not allowed in a Javascript variable name.

#### Example

```javascript
properties: {
	hypotenuse(model__name, model__a, model__b) {
		return `${model__name} has a hypotenuse of ${Math.sqrt(model__a * model__a + model__b * model__b)}`;
	}
}
```

Computed Model Attributes
-------------------------------

Also included with the plugin is the `_addComputed(key, func)` function added to the Model class which can be used to add computed attributes to a model instance. The same argument rules apply to the function but the observable arguments can only be other attributes on the model. 

Change events will trigger for computed attributes. To access a computed attribute on a model use the `get(key)` function. 

To declare computed attributes on a model prototype set the `computed` property.

#### Example

```javascript
computed: {
	hypotenuse(name, a, b) {
		return `${name} has a hypotenuse of ${Math.sqrt(a * a + b * b)}`;
	}
}
```
