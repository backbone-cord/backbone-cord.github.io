classes
-------------------------------

#### Dynamic-Named Classes

CSS class names may be changed dyanmically with interpolation using the value of an observable key. The following example sets the class of a button to the value of key with the myclass- prefix, e.g. `myclass-red` or `myclass-blue`.

#### Example

```javascript
el() {
	return h('button.myclass-[key]');
	// or in JSX
	return <button className="myclass-[key]"></button>;
}
```

#### Conditionally-Applied Classes

A CSS class can be conditionally applied by placing an oberver key in parentheses after the class name to add/remove based on the value.

#### Example

```javascript
h('button.myclass(observerKey)');
// or in JSX
<button className="myclass(observerKey)"></button>;
```

