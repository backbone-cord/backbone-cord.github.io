Loading Plugins
-------------------------------

Most of Cord's functionality is implemented using plugins.  Plugins can specify dependencies but do not implement any kind of dependency management and rely on proper load order from build scripts. There is a one-time requirements check when the first view is initialized to make sure all dependencies are loaded but it will not check if they are loaded in the correct order.

Built-In Plugins
-------------------------------

All built-in plugins are automatically included with the backbone.cord.js distribution file.

* [binding](binding.md) - Binding to observable keys within attributes, text nodes, and input values
* [classes](classes.md) - Dynamic and conditional application of classnames to elements
* [computed](computed.md) - Allows the `get` methods of properties to accept observable keys and to automatically re-compute when those change
* [events](events.md) - Specify `onx` events in the attributes of an element
* [hidden](hidden.md) - A special attribute hidden which will alter the display of the element depending on the value of the observable
* [render](render.md) - Adds a render pipeline for rendering a dynamic part of a view based on arguments
* [styles](styles.md) - Run animations, transitions, and create inline stylesheets from on a dictionary of styles
