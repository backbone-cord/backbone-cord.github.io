Using Scopes
-------------------------------

Scopes that are loaded can simply be used by referring to their scope namespace prefix in the observable keys.

Non-Optional Scopes
-------------------------------

* view - use view properties by simply the property name or `this.propertyname`
* model - use model attributes of the view with `model.attributename`

Built-In Scopes
-------------------------------

All built-in scopes are automatically included with the backbone.cord.js distribution file.

* [sharedscope](sharedscope.md) - Adds a shared model that can be used to globally share variables
* [unmanagedscopes](unmanagedscopes.md) - Alias any model to a new scope

Additional Scopes
-------------------------------

* [clock](https://github.com/backbone-cord/backbone.cord.clock) - Plugin for backbone.cord providing data bindings for both live Date values.
* [input](https://github.com/backbone-cord/backbone.cord.input) - Plugin for backbone.cord providing bindings for user input events
* [storage](https://github.com/backbone-cord/backbone.cord.storage) - Plugin for backbone.cord providing data bindings for both localStorage and sessionStorage.
