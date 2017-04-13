syncing
-------------------------------

The syncing plugin adds a `syncing`, `syncProgress` and `syncError` properties to all views, models, and collections.  The view properties are based on events from the set `model` or `collection` and are observable keys like any other view property.

The `syncError` attribute is set through a callback function defined under `Backbone.Cord.parseError(xhrResponse)`. Override this to parse and return errors from expected responses.
