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

