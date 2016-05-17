About
-------------------------------

**backbone.cord.js** enhances Backbone by enabling the easy creation of reactive connected views with two-way data binding and Jade-like declarative syntax.

#### Requirements

* Backbone 1.1+, other versions and variants may also work but haven't been tested
* Modern Browser, IE9+ - Array.indexOf, Object.getPrototypeOf, Object.defineProperty, Object.getOwnPropertyDescriptor, and Function.bind methods are used, possibly others that would need a polyfill on older browsers

*NOTE:* jQuery and Underscore.js are NOT required, though normally used with Backbone.

Additions to Backbone
-------------------------------

* assumes that each view only has model or collection set, and if a view has collection, then el will reactively render more than one? this moves part of the parent responsibility to the child?
* properties hash is added

#### el

Implement el as a function and it will be passed and _el and _subview methods as arguments. Name these however you want.

```javascript
var MyView = Backbone.View.extend({
	el: function(h, _subview) {
		return h('', {},
			'Text node child',
			h('p', 'A paragraph child. I have {_x} pet {_animal}s.')
		);
	}
	properties: {
		animal: 'dog',
		x: 2
	}
});
```

#### properties

The properties dictionary will synthesize property methods and set their value before el() is called and the layout created. By default properties are synthesized with get/set methods that simply set an internal variable that is the key name prefixed with an _. To override default methods or behavior provide a definition instead of a simple value adhering to the following rules. Each definition is an object made up of ONLY one or more of the keys get, set, and value.

* When definition is just a simple value it implies `{value: definition}` - plain objects should be explicity set using a value key to avoid confusion with a definition
* `set: null` creates a readonly property
* When definition is a function it implies `{get: definition, set: null}`
* When either get or set is missing default accessors that read/write the backing _key are used

#### observers

Similar to properties and events, the observers object describes observer methods to automatically observe various keys and take action when a value changes.

```javascript
var View = Backbone.View.extend({
	//....
	observers: {
		key: 'onKeyChangeViewMethod'
	}
});
```

#### render

The render method will typically go unused, but it could be used for rendering variant views or applying some global state change by rendering some supplied arguments or data.


**NOTE:** Properties are set before the el method is called, so take caution when referencing dom elements from within the set methods.

**NOTE:** Memory leaks can occur when removing elements that make use of binding. Unbinding happens when the containing View is removed. Subviews replaced with other subviews does proper cleanup.

Methods
-------------------------------

`_el(tagIdClasses[, attrs][, children])`

**NOTE:** To insert text that doesn't process bindings and other special synax, simply directly create a text node with document.createTextNode()

`_subview(instanceClass[, idClasses][, events])`

Observing
-------------------------------

`observe(key, observer, immediate)` is added as method to Backbone.View, where a key and observer method can be given to register a callback for any changes. Typically this method is not called directly but indirectly through binding using the binding plugin.

The following key syntax is supported:

* !variable - wraps the callback in a function that will negate the value of variable into a boolean
* %variable - indicates that an immediate callback will happen and observing won't take place - useful as an optimization for variables that won't change - not compatible with !

Builtin to the core is support for observing attributes on the View's model. The keys are just given as keys accessible into the model.  Other scopes are supported through the plugin system.

Using the key 'id' will result in using the model's idAttribute as a key instead.

Reference of What Gets Added
-------------------------------

#### View

#### Model and Collection

When using the syncing plugin, the prototype.sync method gets wrapped and the following properties are added to the object:

Security
-------------------------------

The math plugin can evaluate arbitrary expressions. Be careful to not pass any insecure user-supplied strings directly in as children or atributes on the el method.

License
-------------------------------
MIT
