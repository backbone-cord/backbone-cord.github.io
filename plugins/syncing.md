#### syncing

The syncing plugin adds a `syncing` and `error` attributes to all Views, Models, and Collections.  The View's attributes are based on events from it's model or collection and are observable.

The error attribute is set through a callback function defined under `Backbone.Cord.parseError(xhrResponse)`. Override this to parse errors from expected responses.
