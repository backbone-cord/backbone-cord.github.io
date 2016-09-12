Dataflow
-------------------------------

Backbone.Cord encourages modular views that respect the single responsibility and separation of concerns principles. As a result, Backbone.Cord works on the assumption that view should have a model or a collection but not both. An exception to this is that some collection views may set their model to an item in the collection to indicate a selection.

The model or collection a view manages can be set either on construction or through the `setModel()` or `setCollection()` function. Once a view is established as a model view it should not change to a collection view.

#### The EmptyModel

By default if a Backbone.Cord.View does not have a model set or after calling `setModel(null)` a shared EmptyModel is set on the model. The EmptyModel is special in that the `set()` function is a no-op. The reason for its use is to simplify observing and listening code that can simply assume that a model is always set on the view.

#### setModel()

The `setModel()` function is used to change the model the view is using, where upon changing will invoke all observers set on the existing model. Passing null will remove the current model. Setting the model directly using the view's model property will have no effect and could disrupt expected behavior.

The `setModel(newModel, noCascade)` function takes two arguments, the first being the new model and noCascase, which defaults to false, can be set to true to prevent cascading the model to all subviews.

#### setCollection()

By default, `setCollection()` removes listeners from the current collection and reassigns the collection property. It is provided mostly as a pattern for other plugins to wrap.

#### Cascading

The `setModel()` function will recursively call `setModel()` on all subviews if noCascade is not set to true. Cascading on any particular subview will NOT happen if any of the following are true:

* The subview is not a Backbone.Cord subview
* The subview has a cascade function defined and when called with the new model explicity returns a value of false
* The subview has an existing model that is NOT the same as the current model on the parent. Meaning before `setModel()` is called the parent and subview need to have the exact same model
* The subview is managing a collection

The above conditions are checked in order, which means that the overriding `cascade()` function may be called even if the subview is managing a collection. This allows a model to cascade down into a subview, where the subview loads a related collection and the collection on the subview can change when the model on the parent changes, such as a collection of messages related to a mailbox model.

#### cascade()

The `cascade()` function can be defined on any view to intercept the new model and transform and subsume different data or use a submodel from the new model or set a related collection.

The `cascade(newModel)` function takes only a single argument of the newModel to be set a should return false when cascading was successful.
