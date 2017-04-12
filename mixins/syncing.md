syncing
-------------------------------

The syncing plugin adds a `syncing` and `error` properties to all views, models, and collections.  The view properties are based on events from it's model or collection and are observable keys like any other view property.

The error attribute is set through a callback function defined under `Backbone.Cord.parseError(xhrResponse)`. Override this to parse errors from expected responses.
