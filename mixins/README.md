Applying Mixins
-------------------------------

Mixins automatically merge properties with one or more extendable objects. Properties of the same name that are objects are deeply merged while functions will be chained together in-order.

Built-In Mixins
-------------------------------

All built-in mixins are automatically included with the backbone.cord.js distribution file.

* [collection](collection.md) - Managing a collection with a view providing selections, paging, and automatic sorting
* [syncing](syncing.md) - Adds two properties, 'syncing' and 'error' to the View to indicate when a model or collection is syncing
* [validation](validation.md) - Model validation logic for form fields bound to models with on blur or submit validation
