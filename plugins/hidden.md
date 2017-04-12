hidden
-------------------------------

The hidden plugin simply adjusting the `display` style of any given element to `none` when the observing value changes.

#### Usage

The hidden plugins looks for a named attribute called `hidden`. The value must be a single observable key. Using the ! prefix the hidden state can be inverted.

#### Example

```javascript
h('input', {hidden:'!key'});
// or in JSX
<input hidden="!key"></input>
```

#### invisible

Also supported is the special attribute `invisible`, where based on the same concept as hidden adjusts the element's `visibility` to `hidden` instead of the `display`.
