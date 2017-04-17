svg
-------------------------------

The SVG plugin automatically detects known svg element tags and when the elements are created it uses the appropriate SVG namespace on the tag and attributes. For elements that are not exclusively SVG such as `a`, `iframe`, and `canvas` add either an `svg:` or just `svg` prefix when creating them, e.g. `svgCanvas`.

```javascript
import {View} from "backbone";
import {h} from "backbone.cord";

export default View.extend({
	el() {
		return <svg><circle></circle></svg>;
	}
});
```
