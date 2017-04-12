render
-------------------------------

The render plugin allows the `render` method on a view to be used to dynamically render content based on more complex logic that simple bindings cannot solve. Bindings work as expected inside render and any changes to observed values will invoke a new render and the contents will be updated based on a simple virtual-dom algorithm.

#### Usage

* Binding and observables work inside render
* The render method must return a single element or an array of elements
* The returned value from render will then be added to the DOM appended to the view's root el or a #container element if specified
* Subviews are not allowed inside render
