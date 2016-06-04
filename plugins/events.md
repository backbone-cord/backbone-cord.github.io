#### events

The events plugin allows for easier mapping of all DOM events to a View function. The attributes simply need to be specified as onX, where is X is any event name that can be passed to addEventListener().  Backbone's events hash may also be used not all event listeners need to be created this way.

```javascript
h('button', {onclick: 'viewMethod'});
_subview(Subview, {onclick: 'parentViewMethod'});
```
