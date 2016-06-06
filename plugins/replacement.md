replacement
-------------------------------

The replacement plugin allows for matching against a single DOM node created by the _el hyperscript function and either replace it, augment it, or add siblings to it. 

#### Registering Functions

Replacement functions must be registered globally with the following method: `Backbone.Cord.addReplacement(selector, func)`

* **selector** - Any valid CSS selector
* **func** - Callback function to replace, augment, or add siblings

##### Selectors

The selector MUST include a tag, but otherwise must be any valid selector. The reason being is for faster processing where first tagNames are compared and then only selectors requiring that tagName are run.

##### Callback Arguments

* `el` - The matched element to be replaced or augmented
* `parent` or `fragment` - A temporary documentFragment that can be treated as a parent to the element to add siblings or augment

##### Callback Behavior and Return Values

The callback function must either do nothing or one of the following:

* Return a completely new element, which may also be a new subview's el
* Modifying the first argument and return nothing
* Modify the element and/or add siblings using the documentFragment provided as the second argument

##### Instructions and Restrictions

* Always include a tag in the selector
* Listen to the view's remove event to cleanup anything that is needed
* If replacing an element the old one may still be around with bindings and even as a property through this if an #id is used - be very aware of what the replacement is doing
* DO NOT replace any root elements in a view's el layout
* If the element is the root element for a view and the documentfragment is returned, the remove function will not work properly because the view's el becomes an empty documentfragment

#### Examples

The following shows replacing a psuedo tag with multiple elements.

```javascript
Backbone.Cord.addReplacement('logo', function(el, parent) {
	return this._el('div',
		this._el('h1', '{$brandName}'),
		this._el('img',{src: ''{$brandLogoURL}''}));
});
```

The following shows automatically enabling Pikaday for any inputs with a date type. Also adds a cleanup method when the view is removed.

```javascript
Backbone.Cord.addReplacement('input[type="date"]', function(el) {
	var picker = new Pikaday({ field: el });
	this.once('remove', function() { picker.destroy(); });
});
```

The following adds siblings to an element.

```javascript
Backbone.Cord.addReplacement('div.urgent', function(el, parent) {
	parent.appendChild(this._el('div', 'Get this done now!'));
});
```
