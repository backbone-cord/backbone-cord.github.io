modeltracking
-------------------------------

The modeltracking plugin isn't a normal plugin but a collection of additional functions that get added to Backbone.Model.

#### track(model, attrs, transform)

The track function allows watching the given attributes and setting them on model when they change. Optionally, a transform method can be used to change the tracked attributes or create completely new ones.

* `model` - the model which will be receiving changes
* `attrs` - optional array of attributes to track, otherwise all attributes are tracked
* `transform(data)` - optional transformation function to apply whenever and tracked changes are made before the `set()` function is called on model - should return data
* **Note** - transform may be given as the second argument if attr is left out

#### subsume(cls, attr, transform)

The subsume function enables creating a new model instance based on the attributes of another. The arguments are as follows:

* `cls` - the subsumed model's class to create an instance from
* `attr` - optional attribute to subsume from a subobject on the model
* `transform` - optional transformation function to take the attributes of this model and create attributes of the other model - should return data
* **Note** - transform may be given as the second argument if attr is left out

This utilizes the `track()` function.

#### mirror(model, attrs)

The mirror function simply keeps two model instances in sync whenever one changes, the other gets updated. Optionally, attrs can be specified as an array of keys to listen for and sychronize, otherwise all keys will be synchronized.

This utilizes the `track()` function.
