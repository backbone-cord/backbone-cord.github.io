storage
-------------------------------

The storage scope provides binding access to the root levels of both `localStorage` and `sessionStorage`. The namespace used is also the same `localStorage` and `sessionStorage`.

* When setting a value through localStorage the change will be observed on all windows including the current one.
* When setting a value through sessionStorage the change will be observed on all windows (iframes) within the current window and including the current window itself.

<https://github.com/backbone-cord/backbone.cord.storage>
