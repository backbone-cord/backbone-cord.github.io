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

In Backbone, normally the el function returns the view's root element but in Cord this is used to declare the entire layout including the root with bindings etc. This can be specified with JSX or manually using the h function.

```javascript
import {View} from "backbone";
import {h} from "backbone.cord";

export default View.extend({
	el() {
		return <p>Hello World. <b>This is the layout!</b></p>;
	}
});
```

#### model

Each view always manages a single model. If not model is given a readonly empty model is used. The model can be set on a view in a variety of ways:

* on initialize by passing the `model` option
* after initialization with the `setModel()` method - important to use the method because it invokes model observers
* set automatically to the parent's model when adding as a subview
* give a model class on the view's prototype and a new instance of that model will be created before initialization
* implement a view method called `cascade(model)` which intercepts the automatic setting from the parent's model, call `setModel` with some related model instead and return `false`

```javascript
import {View} from "backbone";
import {h} from "backbone.cord";
import NameModel from "NameModel";

export default View.extend({
	model: NameModel,
	el() {
		return <p>Hello [model.firstName] [model.lastName]!</p>;
	}
});
```

#### properties

The properties dictionary will synthesize property methods and set their value before the layout is created. By default properties are synthesized with get/set methods that simply set an internal variable that is the key name prefixed with an _. To override default methods or behavior provide a definition instead of a simple value adhering to the following rules. Each definition is an object made up of ONLY one or more of the keys: `get`, `set`, and `value`.

The following shortcuts are available:

* When the definition is just a simple value it implies `{value: definition}` - plain objects should be explicity set using a value key to avoid confusion with a definition
* Include `readonly: true` in the definition or `set: null` to create a readonly property where settings the value can only be done with a `new Backbone.Cord.ForceValue(value)`
* When the definition is just a function it implies `{get: definition, set: null}`
* When either get or set is missing default accessors that read/write the backing _key are used

```javascript
import {View} from "backbone";

export default View.extend({
	properties: {
		a: {
			set(value) {
				this._a = value;
				// ... use a for something 
			}
		}
		x: () => 123,
		y: 123,
		z: {
			value: 456,
			readonly: true
		}
	}
});
```

**id'ed** elements and subviews are automatically provided as view properties for easy access within the view.  For example:

```javascript
import {View} from "backbone";
import {h} from "backbone.cord";
import MySubview from "MySubview";

export default View.extend({
	el() {
		return <p id="welcome">Welcome! <MySubview id="mysubview"></MySubview></p>;
	},
	//... this.welcome is set to the <p> DOM element and this.mysubview is set to the instance of MySubview
	//... this.mysubview can be assigned a new subview and the DOM will be updated, however this.welcome cannot be reassigned
});
```

For computed properties also see [computed properties](plugins/computed.md).

#### observers

The observers dictionary is a shorthand to call the `observe(key, callback)` before the view is initialized.

```javascript
var MyView = View.extend({
	//....
	observers: {
		propName() {
			console.log(`${this.propName} has changed!`);
		}
	}
});
```

#### styles

The styles dictionary can be used to provide scoped styles just for the view. See [the styles plugin](plugins/styles.md) for more details.

#### render

Optionally used to render content with more complicated logic through virtual-dom updates. See [the render plugin](plugins/render.md) for more details.

Data Binding
-------------------------------

`observe(key, observer, immediate)` and `unobserve(key, observer)` are added as methods to the View class, where a key and observer method can be given to register/unregister a callback for any changes. Typically this method is not called directly but indirectly through binding using the binding plugin.

The default interpolation format is to place a key between two brackets, e.g. `[key]`, but these are configurable by setting `Backbone.Cord.regex.variable` for example, `regex.variable = {prefix: '[', suffix: ']'};`

### Scopes

Every observable key must include a scope namespace, which is a `name.` prefix given before every key. The only exception is as a shortcut the view scope which references declared view properties can omit the scope namespace which is defined as `this.`.

## Key Syntax

The following key syntax is supported:

* !key - wraps the callback in a function that will negate the value into a boolean
* %key - indicates that only an immediate callback will happen and observing changes won't happen
* key|filter - runs the value through one or more filters
* key.subkey - subkeys can be used but only observes the first level key for changes not when the subkey changes

**NOTE:** Using any of the above special syntax (not just a single key) will mean that unobserve() is not usable because the observer function becomes wrapped.

#### filters

Using the pipe character, reusable filter functions can be applied to values before they are passed to the end observer. Custom filters can be added through `Backbone.Cord.filters`.

Built-in filters include:

* `lower` - Convert to a lowercase value
* `upper` - Convert to a uppercase value
* `title` - Convert to a titlecase value with each word capitalized

**NOTE:** All Math functions are also a default fallback when registered filter does not match, for example `floor` and `ceil` both are valid filters.

```javascript
import {View} from "backbone";
import {h, filters} from "backbone.cord";

filters.len = (str) => str.length;
export default View.extend({
	el() {
		return <p>Hello [model.name|upper], your name is [model.name|len] long!</p>;
	}
});
```

#### subkeys

Using dot notation, values can be narrowed down to any level of nested subkey before being passed to the end observer. But as noted above only the top-level key is actually observed for changes. The following subkeys have special meaning:

* `.id` is an alias to the model's idAttribute
* `.value` can be used on an id'ed DOM element to get the decoded value from it

## Observer Syntax

The following observer syntax is supported in addition to providing a normal callback function:

* key into the view that is set to a function to use as a callback
* a key using the key syntax above which can then be used to call `setValueForKey()` with the observed value - used to proxy on value to another
* subviewkey.property - for id'ed subviews deep nesting with the subkey dot syntax into the propery of a subview

#### Getting/Setting Values with Key Syntax

Also included is a standard way to set and get values from any scope associated with the view.

`getValueForKey(key)` - key supports the same key syntax as above
`setValueForKey(key, value)` - key supports the same observer syntax as above
`setValuesForKeys(keyValues)` - provide a dictionary of values or arguments given as key1, value1, key2, value2, etc...

NOTE: If the key does not exist for a key it will simply return undefined.

License
-------------------------------
MIT
