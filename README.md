About
-------------------------------

**backbone.cord.js** enhances Backbone by enabling the easy creation of reactive connected views with two-way data binding and Jade-like declarative syntax.

**backbone.cord.js** is a layout and data pipeline for Backbone that connects views with subviews and models. Observation and plugin system also enables many types of reactive interfaces.

#### Requirements

* Backbone 1.1+, other versions and variants may also work but haven't been tested
* Modern Browser, IE9+ - Array.indexOf, Array.isArray, Object.getPrototypeOf, Object.defineProperty, Object.getOwnPropertyDescriptor, and Function.bind methods are used, possibly others that would need a polyfill on older browsers

*NOTE:* jQuery and Underscore.js are NOT required, though normally used with Backbone.

#### Compatibility Mode

By default, Cord will globally modify the Backbone.View class. However, if `window.cordCompatibilityMode = true;` is set before Cord is loaded then Backbone.View will be extended as Backbone.Cord.View and all view modifications will happen on that class and Backbone.View will remain untouched. Backbone.View and Backbone.Cord.View are compatible with each other. There is no compatibility mode for Backbone.Model - any plugin changing Backbone.Model applies to all models.

Additions to Backbone
-------------------------------

* assumes that each view only has model or collection set, and if a view has collection, then el will reactively render more than one? this moves part of the parent responsibility to the child?
* properties hash is added
* observers hash is added

#### el

Implement el as a function and it will be passed and createElement and createSubview methods as arguments. Name these however you want. Below h and s are used as abbreviations of hyperscript and subview.

```javascript
var MyView = Backbone.View.extend({
	el: function(h, s) {
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

Observers
-------------------------------

Similar to properties and events, the observers object describes observer methods to automatically observe various keys and take action when a value changes.

```javascript
var View = Backbone.View.extend({
	//....
	observers: {
		key: 'onKeyChangeViewMethod'
	}
});
```

The key/value pairs in the observers hash follow the same syntax described under Observing.

#### filters

Using the pipe character, reusable filters can be applied to values before they are passed to the end observer.

Built-in filters include

* `lower` - Convert to a lowercase value
* `upper` - Convert to a lowercase value
* `title` - Convert to a titlecase value with each word capitalized

All Math functions are also a default fallback when registered filter does not match, for example `floor` and `ceil` both are valid filters.

#### subkeys

Using dot notation, values can be narrowed down to any nested subkey before being passed to the end observer.

#### model and collection

These properties behave as normally, but to make changes to them use the supplied `setModel()` and `setCollection()` functions. 

model and collection may be specified on the view's prototype as classes and a new instance of the model or collection will automatically be created when a new view is instantiated.

#### render

The render method will typically go unused, but it could be used for rendering variant views or applying some global state change by rendering some supplied arguments or data.


**NOTE:** Properties are set before the el method is called, so take caution when referencing dom elements from within the set methods.

**NOTE:** Memory leaks can occur when removing elements that make use of binding. Unbinding happens when the containing View is removed. Subviews replaced with other subviews does proper cleanup.

Order of Instantiating
-------------------------------

Within Backbone's view constructor the following things happen in this order.

1. The wrapped `_ensureElement()` is called where the following happens before `initialize()`
2. If model or collection are specified on the prototype as a class, then they are replaced with an instance of the model or collection
3. All properties are synthesized
4. If el is a function it is bound and partially applied with createElement and createSubview arguments
5. All observers are setup
6. `initialize()` is called
7. `render()` if needed is called outside by the parent view or whatever code created the view
8. Value observers requiring initial values are called using a setTimeout

Differences to Creating a View's Element with Backbone
-------------------------------

Normally, in Backbone, either an element is provided as an existing element with the el attribute, or one is created with the given tagName, id, and className, or a function is used to generate the element.  Cord Views normally use the latter with presupplied arguments for creating hyperscript and subviews.  Also, when the el is function Cord will apply the className to the el which Backbone normally does not do.

Methods
-------------------------------

`createElement(tagIdClasses[, attrs][, children])`

**NOTE:** To insert text that doesn't process bindings and other special synax, simply directly create a text node with document.createTextNode()

`createSubview(instanceClass[, idClasses][, bindings])` - where bindings is an object of events names mapping to a function or a string using the observer syntax below.  If an event is set to map to a property, the last argument of the event callback will be used as the value.

Observing
-------------------------------

`observe(key, observer, immediate)` is added as method to Backbone.View, where a key and observer method can be given to register a callback for any changes. Typically this method is not called directly but indirectly through binding using the binding plugin.

The following key syntax is supported:

* !variable - wraps the callback in a function that will negate the value of variable into a boolean
* %variable - indicates that an immediate callback will happen and observing won't take place - useful as an optimization for variables that won't change - not compatible with !
* variable|filter - runs the variables
* variable.subkey - observes only the first level key - doesnt' suport nested models

**NOTE:** Using any of the above special syntax (not just a single key) will mean that unobserve() is not usable because the observer function is wrapped.

Builtin to the core is support for observing attributes on the View's model. The keys are just given as keys accessible into the model.  Other scopes are supported through the plugin system.

Using the key 'id' will result in using the model's idAttribute as a key instead.

The following observer syntax is supported in addition to a normal function:

* key for a function on the view to use as a callback
* key used to call setValueForKey()
* _subviewkey.property - only supports subviews chains for deep nesting with the dot syntax nested not properties of properties. 

Getting/Setting Values
-------------------------------

Also included is a standard way to set and get values from any scope associated with the view.

`getValueForKey(key)` - key supports the same key syntax as above
`setValueForKey(key, value)` - key supports the same observer syntax as above

NOTE: If the key does not exist for a key it will simply return undefined.

License
-------------------------------
MIT
