events
-------------------------------

The events plugin allows for easier mapping of all DOM events to a view function. The attributes simply need to be specified as `onx`, where is x is any event name that can be passed to `addEventListener()` and the value of `onx` is a function or a string indicating a function name on the view. Event listeners can also be added the same way to the root elements of subviews throught the `_subview(` function. Backbone's events hash may also be used not all event listeners need to be created this way.

#### Example

```javascript
h('button', {onclick: 'viewMethod'});
_subview(Subview, {onclick: 'parentViewMethod'});
```
#### onkeyup and focus

The keyup event can be added to any child but in order for the events to trigger it must first be focused. If the element is not a typical input element a special `focus(childId)` method, which has been added to `Backbone.Cord.View`, must first be called.
