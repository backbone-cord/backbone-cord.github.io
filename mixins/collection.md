collection
-------------------------------

The collection mixin manages a `collection` on the view instead of a `model`. Similar to a normal view the collection should be set on the initialize options or via the `setCollection` method. Each item in the collection is shown in a subview created by the class specified on the `itemView` attribute and added either to the root element of the collection view or to an id'ed `#container` element. Each subview has the model set automatically when added using the `setModel` function. If the `collection` attribute is set on the view's prototype a new collection of that type will be automatically instantiated for the view.

#### Properties

The following properties are added to any view with the collection mixin:

* `length` - readonly value representing the length of the collection
* `start` - readonly value of the 0-based index of the starting paged position in the collection
* `end` - readonly value of the 0-based index of the ending paged position in the collection
* `pageStart` - set to 0-based index of where the display of the collection should start
* `pageLength` - set to how many items to show per page, -1 is show all
* `selected` - set to the Model representing the current selection. For binding and cascading to non-item subviews `this.model` is also set to the selected

To show an empty message when the collection is empty simply use the hidden plugin set to the `length` key.

#### Collection Events

When the collection object is `reset`, `sort`, or items change via `add` or `remove`, the item subviews will automatically be synchronized.

#### Subview Events

If any of the item subviews remove themselves the corresponding item in the collection will also be removed.

#### Example

```javascript
import {View} from "backbone";
import {h, filters} from "backbone.cord";

filters.plusOne = (value) => { return value + 1; };

const TodoItemView = View.extend({
	el() {
		return <li>[model.value]
			<a href="#" onclick={this.remove.bind(this)}>(x)</a>
		</li>;
	}
});

const TodoView = View.extend({
	mixins: ['collection'],
	itemView: TodoItemView,
	el() {
		return <div>
			<h3>TODO</h3>
			<ul id="container"></ul>
			<form onsubmit="_onSubmit">
				<input id="todo" placeholder="Your next todo"></input>
				<button>Add #[length|plusOne]</button>
			</form>
		</div>;
	},
	_onSubmit() {
		this.collection.add({value: this.todo.value});
		this.todo.value = '';
		return false;
	}
});
```
