replacement
-------------------------------

The replacement plugin allows for matching against a single DOM node created by the createElement hyperscript function and either replace it, augment it, or add siblings to it. 

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

#### Excluding Elements from Replacement

Sometimes automatically replacing matched elements is not the desired behavior, for example when replacing one element with view containing the same element type, its `h()` function could invoke the replacement recursively.

To exclude an element from replacement functions simple add the attribute `data-noreplace`.

#### View Local Replacements

Instead of registering a replacement function globally they can be provided within the scope of a view only. This is useful for building mixins or parent classes that need to control the building of a subclasses layout. To provide local replacement functions use the same syntax as global replacement functions but instead supply it on the view prototype level under the `replacements` property. The replacements must also be compiled using `Backbone.Cord.compileReplacements()`. See below for an example.

#### Examples

The following shows replacing a psuedo tag with multiple elements.

```javascript
Backbone.Cord.addReplacement('logo', function(el, parent) {
	return this.createElement('div',
		this.createElement('h1', '{$brandName}'),
		this.createElement('img',{src: ''{$brandLogoURL}''}));
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
	parent.appendChild(this.createElement('div', 'Get this done now!'));
});
```

The following creates local replacement functions.

```javascript
Backbone.Cord.mixins['example'] = {
	replacements: Backbone.Cord.compileReplacements({
		'logo': function(el, parent) {
				return this.createElement('div',
					this.createElement('h1', '{$brandName}'),
					this.createElement('img',{src: ''{$brandLogoURL}''}));
		}
	})
}
```
