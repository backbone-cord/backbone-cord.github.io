styles
-------------------------------

The styles plugin can apply styling to a whole view (non-subview) DOM heirarchy in a View and it can also apply styling to a single element.

Single element styling is done using the special style attribute and is applied directly to the elements style javascript object. The example below creates a green button 100px wide. 

```javascript
h('button', {style: {color: 'white', width: '100px', backgroundColor: 'green'}});
```

Styling the entire layout can be accomplished with a style object on the view.

NOTE: A className must be provided on the view in order to create dynamic stylesheets

NOTE: ALL style values MUST be a string, integers are not allowed

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

#### animations

Animations are used to temporarily change styles of elements for visual feedback and rely on pre-defined keyframes

The animations dictionary is given as keys mapping to either an array or a dictionary with keyframe keys such as 50%. When using an array the keyframes will be equally divided into percentages. An array with a single entry will map to an empty 0% keyframe and 100% keyframe with the styles. When using a dictionary both a 0%/100% or to/from keys should be provided.

Default `options` can be given in the animation dictionary but will be overriden by any user-provided options.

Even though beginAnimation can start many animations together all animations will combine the defaults, animation defaults, and user-provided args into a single options object.

Returning false from the callback on each iteration will pause the animation before being resumed, useful for animating forward then reverse using count of 2 and direction of alternate

NOTE: The selectors used can descend into subviews potentially selecting elements not wanted to select. 

#### transitions

Used to do one-off permanent altererations to the styles of elements. 

NOTE: The selectors used can descend into subviews potentially selecting elements not wanted to select. 
