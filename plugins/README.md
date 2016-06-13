Loading Plugins
-------------------------------

Most of cord's functionality is implemented using plugins.  Plugins can specify dependencies but do not implement any kind of dependency management and rely on proper load order or concatenation order in build scripts. There is a one-time requirements check when the first view is initialized to make sure all dependencies are loaded but it will not check if they are loaded in the correct order.

Loading is done by calling `push` or `unshift` on the array-like object Backbone.Cord.plugins.

List of Built-in Plugins
-------------------------------

All built-in plugins are automatically included and loaded when using the provided Backbone.Cord distribution file.

* **binding** - Binding and reverse bind to observable keys within attributes, text nodes, and input events
* **collection** - Managing a view with a collection with selections and paging
* **computed** - Allows the `get` methods of properties to accept observable keys and to automatically re-compute when those change
* **conditionalclasses** - Apply or remove a css class based on the boolean value of a variable
* **dataid** - Changes all of the id attributes into data-id attributes, so they may be reused among different instance of the same View
* **dynamicclasses** - Bind part or all of an elements css classname based on the value of a variable
* **events** - Specify onX events in the attributes dictionary of an element
* **hidden** - A special attribute hidden which will alter the display of the element depending on the boolean value of the variable
* **interpolation** - Adds the method observeFormat to Backbone.View, where a formatted string of variables is observer instead of a single
* **math** - Create mathematical expressions that can be bound
* **modeltracking** - Adds track, subsume, and mirror methods to Backbone.Model
* **render** - Adds a render pipeline for rendering a dynamic part of a view based on static data
* **replacement** - Globally add query strings that can match and replace or augment nodes created through
* **styles** - Dynamically creates stylesheets or observes variables based on a dictionary of styles
* **syncing** - Adds two properties, 'syncing' and 'error' to the View to indicate when a model or collection is syncing
* **scopes** - Variable scopes
	* **shared** - Adds a shared model that can be used to globally share variables
	* **view** - Scope, where properties on the view become variables that can be observed

Authoring Plugins
-------------------------------

#### Overview

#### Dependencies

Dependencies can be specified as the key `requirements`, an array of strings, inside the plugin and Cord will automatically check to make sure plugins wih those names are also registered.  It does not however check the order of plugins.  To ensure a plugin is run before others use the unshift method.

#### Plugin Specification

Plugins define a name, requirements, any number of callbacks, and then register themselves either by pushing or unshifting one plugin at a time to the plugins array.

* `tag(context, tagName)` - (_el) process a tagName and optionally return an element overriding the default `createElement(tagName)`
* `classes(context, classes)` - (_el and _subview) classes is an array of class names, altering classes or returning a different array will prevent default classes being applied
* `attrs(context, attrs)` - (_el but conditionally invoked) attrs is a dictionary, altering attrs or returning a different dictionary will prevent default attrs being applied
* `children(context, children)` - (_el conditionally invoked) children is an array of strings or dom elements
* `bindings(context, bindings)` - (_subview) bindings that by default get converted to event listeners 
* `complete(context)` - (_el and _subview) when creation and setup is complete, right before el and subview return, returning a different element can replace the el completely
* `create(context)` - (new Cord.View()) called before properties are synthesized
* `initialize(context)` - (new Cord.View()) called before observers are setup
* `remove(context)` - called before a Cord.View is removed with `remove()`
* `strings(context, strings)` - plugin callback to be used adhoc for processing strings from other plugins, where strings is a dictionary

#### Scope Plugin Specification

Plugins can also provide a variable binding scope for to allow access to other variables outside of the View's model. All that is needed is a `scope` object specified in the plugin with the following methods:

* `getKey(key)` - given a raw key, return the internal actual key this scope uses internally. For example, the viewscope looks for a prefix of _ and returns the key without that prefix that will be used as the first argument to every other scope method
* `observe(key, observer, immediate)` - a callback before the observer is added to the observer list
* `unobserve(key, observer)` - a callback after the observer is removed from the observer list
* `getValue(key)` - return the value given a key
* `setValue(key, value)` - set a value given a key
