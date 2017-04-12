About
-------------------------------

**backbone.cord.js** is a Backbone library for data binding and a JSX-compatible declarative layout pipeline.

#### Requirements

* Backbone 1.1+, other versions and variants may also work but haven't been tested
* Modern Browser, IE9+ - Array.indexOf, Array.isArray, Object.getPrototypeOf, Object.defineProperty, Object.getOwnPropertyDescriptor, and Function.bind methods are used, possibly others that would need a polyfill on older browsers
* jQuery and/or Underscore.js are NOT required, though normally used with Backbone.

#### Compatibility Mode

By default, Cord will globally modify the Backbone.View class. If `window.cordCompatibilityMode = true;` is set before Cord is loaded then Backbone.View will remain untouched and only Backbone.Cord.View will be upgraded.

#### Documentation

* [api](api) - internals of the library and how to create plugins
* [mixins](mixins) - behavioral mixins for views
* [patterns](patterns) - code and data structuring conventions
* [plugins](plugins) - plugins that alter the pipeline of how elements are created
* [scopes](scopes) - additional variable and model scopes for data binding

Views
-------------------------------

#### el

In accordance with Backbone, the el function returns the view's layout. This can be specified with JSX or manually using the h function.

```javascript
import {View} from "backbone";
import {h} from "backbone.cord";

export default View.extend({
	el() {
		return <p>Hello [model.firstName] [model.lastName].</p>;
	}
});
```

#### model

The model on the view can be set in a variety of different ways:

* set automatically on view creation by setting the `model` class attribute to a model class
* set by the `setModel()` method
* cascaded by the parent's model automatically when adding as a subview
* not set at all and default to an empty model that cannot be changed


#### properties

The properties dictionary will synthesize property methods and set their value before el() is called and the layout created. By default properties are synthesized with get/set methods that simply set an internal variable that is the key name prefixed with an _. To override default methods or behavior provide a definition instead of a simple value adhering to the following rules. Each definition is an object made up of ONLY one or more of the keys get, set, and value.

* When definition is just a simple value it implies `{value: definition}` - plain objects should be explicity set using a value key to avoid confusion with a definition
* `set: null` creates a readonly property
* When definition is a function it implies `{get: definition, set: null}`
* When either get or set is missing default accessors that read/write the backing _key are used

#### observers

#### styles

The styles attribute is a nested object of CSS rules that follow direct parent child relationships. Each rule must be given as the Javascript camelCase version but browser prefixing is not required. Each value must be a string.

Observers
-------------------------------

Similar to properties and events, the observers object describes observer methods to automatically observe various keys and take action when a value changes.

```javascript
var MyView = View.extend({
	//....
	observers: {
		propName() {
			console.log(`{this.propName} has changed!`);
		}
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

Differences to Creating a View's Element with Backbone
-------------------------------

Normally, in Backbone, either an element is provided as an existing element with the el attribute, or one is created with the given tagName, id, and className, or a function is used to generate the element.  Cord Views normally use the latter with presupplied arguments for creating hyperscript and subviews.  Also, when the el is function Cord will apply the className to the el which Backbone normally does not do.

Methods
-------------------------------

`createElement(tagIdClasses[, attrs][, children])`

**NOTE:** To insert text that doesn't process bindings and other special synax, simply directly create a text node with document.createTextNode()

`createSubview(instanceClass[, idClasses][, bindings])` - where bindings is an object of events names mapping to a function or a string using the observer syntax below.  If an event is set to map to a property, the last argument of the event callback will be used as the value.

**NOTE:** To create a global alias to createElement such as window.createElement etc. be sure to bind it to the Cord object `window.createElement = Backbone.Cord.createElement.bind(Backbone.Cord);`

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
