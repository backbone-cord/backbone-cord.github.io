styles
-------------------------------

The styles plugin can apply styling to a whole view (non-subview) DOM heirarchy in a View and it can also apply styling to a single element.

Single element styling is done using the special style attribute and is applied directly to the elements style javascript object. The example below creates a green button 100px wide. 

```javascript
h('button', {style: {color: 'white', width: '100px', backgroundColor: 'green'}});
```

Styling the entire layout can be accomplished with a style object on the view.

NOTE: A className must be provided on the view in order to create dynamic stylesheets

```javascript
Backbone.View.extend({
	className: '',
	styles: {
		backgroundColor: '#DDD',
		border: '2px dotted #CCC',
		boxShadow: '1px 2px 2px black',
		'#container': {
			borderLeft: '10px solid black'
		}
	}
});
```

The following top-level dictionaries can be used to create media queries:

* mobile - Screens less than or equal to 320px wide
* phablet - Screens less than or equal to 480px wide
* tablet - Screens less than or equal to 768px wide
* desktop - Screens less than or equal to 992px wide
* hd - Screens less than or equal to 1200px wide
