hidden
-------------------------------

The hidden plugin is very simple and serves the purpose of automatically adjusting the display style of any given element to none when the observing value changes.

#### Usage

The hidden plugins looks for a special attribute called "hidden" on the attributes passed through the _el function or a special binding called "hidden" passed through on the _subview function. The value must be a single observable key. Using the ! prefix the hidden state can be inverted.

#### Example

```javascript
h('input', {hidden:'!varName'});
```
