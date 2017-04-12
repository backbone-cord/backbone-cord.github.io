events
-------------------------------

The events plugin allows for easier mapping of all DOM events to a view or callback function. The attributes simply need to be specified as `onx`, where is x is any event name that can be passed to `addEventListener()` and the value of `onx` is a function or a string indicating a function name on the view. Returning `false` from the callback function will result in both `preventDefault` and `stopPropagation` being called on the event.

#### Example

```javascript
h('button', {onclick: 'viewMethod'});
// or in JSX
<Subview onclick="parentViewMethod"></Subview>;
```
#### onkeyup and focus

The keyup event can be added to any child but in order for the events to trigger it must first be focused. If the element is not a typical input element a special `focus(childId)` method, which has been added to `Backbone.Cord.View`, must first be called.
