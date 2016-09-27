#### Inherited Property Values

To tell a property to inherit from another global or other value until it has its own value use the following code.  For example, this is useful for creating a global theme.  The computed plugin is required to make this work.

```
Backbone.View.extend{
	properties: {
		tint: function($tint) { return this._tint || $tint; }
	}
};
```
