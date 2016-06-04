#### math

The math plugin creates a special binding syntax :=expression=:, that can bind to all the variables provided in the expression and update through a single expression property on the View.

```javascript
h('', 'The variable x is :=100 - {x}=: less than one hundred.');
```