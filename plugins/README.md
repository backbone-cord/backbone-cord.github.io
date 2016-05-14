Builtin Plugins
-------------------------------

Most of cord's functionality is implemented using plugins.  Plugins do not implement any kind of dependency management and rely on proper load order or concatenation order in build scripts. Although there is a one-time requirements check when the first View is initialized.

* **binding** - bindings within attributes and text nodes
* **collection** - managing a view with a collection
* **conditionalclasses** - Apply or remove a css class based on the boolean value of a variable
* **dataid** - Changes all of the id attributes into data-id attributes, so they may be reused among different instance of the same View
* **dynamicclasses** - Bind part or all of an elements css classname based on the value of a variable
* **events** - Specify onX events in the attributes dictionary of an element
* **hidden** - A special attribute hidden which will alter the display of the element depending on the boolean value of the variable
* **interpolation** - Adds the method observeFormat to Backbone.View, where a formatted string of variables is observer instead of a single
* **math** - Create mathematical expressions that can be bound
* **reversebinding** - Set values based on input or change events from an input element
* **styles** - Dynamically creates stylesheets or observes variables based on a dictionary of styles
* **syncing** - Adds two properties, 'syncing' and 'error' to the View to indicate when a model or collection is syncing
* **scopes** - Variable scopes
	* **shared** - Adds a shared model that can be used to globally share variables
	* **view** - Scope, where properties on the view become variables that can be observed


Plugin Examples
-------------------------------

#### binding

Binds element attributes and text-based children to variables.

```javascript
_el('div', {data-name: '{name}'}, 'Example with name bound here: {name}.');
```

*NOTE:* Any DOM attribute can be bound to a variable or a formatted string, however it is recommended to NOT use the style attribute in this way because any styles applied with javascript, like with the hidden plugin will get lost when the style attribute is set on a value getting updated.

#### binding (reverse)

Reverse binding can be done with special `input` or `change` attributes, each corresponding to those triggered events.

```javascript
_el('input', {change:'varName'});
```

#### hidden

```javascript
_el('input', {hidden:'!varName'});
```

#### events

The events plugin allows for easier mapping of all DOM events to a View function. The attributes simply need to be specified as onX, where is X is any event name that can be passed to addEventListener().  Backbone's events hash may also be used not all event listeners need to be created this way.

```javascript
_el('button', {onclick: 'viewMethod'});
_subview(Subview, {onclick: 'parentViewMethod'});
```

#### conditionalclasses

A CSS can be conditionally applied using the following syntax.

```javascript
_el('button.myclass(varName)');
```

#### dynamicclasses

A CSS class may be changed dyanmically using the value of a variable. For example, the following may set the class of button to any value of varName with the myclass- prefix, e.g. myclass-red, myclass-blue

```javascript
_el('button.myclass-{varName}');
```

#### styles

The styles plugin can apply styling to a whole view (non-subview) DOM heirarchy in a View and it can also apply styling to a single element.

Single element styling is done using the special style attribute and is applied directly to the elements style javascript object. The example below creates a green button 100px wide. 

```javascript
_el('button', {style: {color: 'white', width: '100px', backgroundColor: 'green'}});
```

Styling the entire layout can be accomplished with a style object on the view.

NOTE: A className must be provided on the view in order to create dynamic stylesheets

```javascript
Backbone.View.extend({
	className: '',
	styles: {
		backgroundColor: '#DDD',
		border: '2px dotted #CCC',
		boxShadow: '1px 2px 2px black',
		'#container': {
			borderLeft: '10px solid black'
		}
	}
});
```

The following top-level dictionaries can be used to create media queries:

* mobile
* phablet
* tablet
* desktop
* hd

#### math

The math plugin creates a special binding syntax :=expression=:, that can bind to all the variables provided in the expression and update through a single expression property on the View.

```javascript
_el('', 'The variable x is :=100 - {x}=: less than one hundred.');
```

#### syncing

The syncing plugin adds a `syncing` and `error` attributes to all Views, Models, and Collections.  The View's attributes are based on events from it's model or collection and are observable.

The error attribute is set through a callback function defined under `Backbone.Cord.parseError(xhrResponse)`. Override this to parse errors from expected responses.
