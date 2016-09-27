General Patterns
-------------------------------

#### Aliasing Utility Functions

The following functions can be conveniently mixed in to underscore.js with the following code. They can also be aliased to whatever names you like.

```
_.mixin({
	isPlainObj: Backbone.Cord.isPlainObj,
	copyObj: Backbone.Cord.copyObj,
	mixObj: Backbone.Cord.mixObj
});
```
