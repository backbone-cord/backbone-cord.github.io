Exports
-------------------------------

The Cord object is the default export but as part of the process Cord will modify the default react/preact module with the following Cord values:

* `Component` - replaced with an extended Component for managing observers with the lifecycle
* `bind(key)` - Function to use within a `render()` method where an observable key is returned immediately and an observer is added to rerender on any changes to the key
* `computed(func)` - Used to create computed state that changes when the named key arguments of func change. Use this inside a Component's constructor when setting the first initial state object.  e.g. `this.state = { c: computed(function(one, two) { return one + two; }) };`

JSX Interpolation
-------------------------------

All text children in JSX are checked for binding interpolation.  Anything between `[key]` subscripts is interpolated as a call to bind(). To avoid having subscripts interpolated place a `raw` attribute on the element.

Component
-------------------------------

The base Component is extended to include all `Backbone.Events` methods and `Cord.Binding` methods. Cord.Binding includes the following:

* `observe`
* `unobserve` 
* `setValueForKey` 
* `getValueForKey` 
* `setValuesForKeys` 
* `setModel` 
* `setCollection`

The same observer syntax and key syntax are used as documented in the base [..](README).

Binding Attributes
-------------------------------

The following attributes on JSX elements are supported:

* `bind` -  shortcut for `observe` + `change`
* `observe` - set the value for this element whenever the key changes
* `change` - set the value for the key whenever the `change` event occurs on the element
* `input` - set the value for the key whenever the `input` event occurs on the element

**IMPORTANT:** -  In order for data-binding to function a key or name attribute is required as a way to uniquely identify an element.

**NOTE:** - These attributes are compatible with `oninput` and `onchange` attributes, the methods will simply be chained together.

Mixins
-------------------------------

The following mixins are included:

* `syncing` - automatically manages `syncing`, `syncProgess`, and `syncError` as vales under the state
* `validation` - validate a model bound to form fields before submitting. Automatically manages `allErrors`, `latestError`, and `isValid` in the state
* `validateOnBlur` - mixin to validate anytime any change to the model happens

Additional mixins can be added to Cord.mixins. To use mixins, simply add a mixins array of strings on a component prototype.

**IMPORTANT:** - In order for validation to happen onsubmit event must be added to the form in question. e.g. `onsubmit: this.validateForm.bind(this)`

Scopes
-------------------------------

All Cord scopes will work exactly the same under React. The default "this" scope maps to a components state object and makes appropriate calls to setState when a key changes.

* `error` - when using the `validation` mixin a model `this.errors` is added to the component and this scope contains errors that match attribute keys from `this.model`
* `model` - observes all the keys on each component's `this.model`.  The `this.setModel()` function must be used in order for this scope to work.
* `shared` - a default globally shared model that can store any value
* `route` - global scope that stores path components when using the included Router

Additional scopes can be added directly to Cord.scopes or for globally accessible shared models they can easily be exposed through SharedScopes, e.g. `Cord.SharedScopes.set('user', new UserModel());`

Models and Router
-------------------------------

Also included with the module is Cord extensions to `Backbone.Model` and the `Backbone.Router`.

* The extended Model adds support for [../plugins/computed.md](computed) attributes
* The Router subclass exposes components of the browser's URL route into a route scope
