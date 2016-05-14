Creating Plugins
-------------------------------

#### Specification

Plugins define a name, requirements, any number of callbacks, and then register themselves either by pushing or unshifting one plugin at a time to the plugins array.

Dependencies can be specified as the key `requirements`, an array of strings, inside the plugin and Cord will automatically check to make sure plugins wih those names are also registered.  It does not however check the order of plugins.  To ensure a plugin is run before others use the unshift method.

#### Scope Specification

Plugins can also provide a variable binding scope for to allow access to other variables outside of the View's model. All that is needed is a `scope` object specified in the plugin with the following methods:

* `getKey(key)` - given a raw key, return the internal actual key this scope uses internally. For example, the viewscope looks for a prefix of _ and returns the key without that prefix that will be used as the first argument to every other scope method
* `observe(key, observer, immediate)` - a callback before the observer is added to the observer list
* `unobserve(key, observer)` - a callback after the observer is removed from the observer list
* `getValue(key)` - return the value given a key
* `setValue(key, value)` - set a value given a key
