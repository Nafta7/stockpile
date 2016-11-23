# jQuery like functionality

```js
/**
 * alias document.querySelector
 * -
 *   the .bind(document) part will set `this` within document.querySelector to
 *   reference `document`.
 */
var _$ = document.querySelector.bind(document)

// DRY. Caching values in variables prevents multiple lookups on the DOM.
// Also makes checking values easier to read.
var violation = _$('#violationSel').value
var status = _$('#statusSel').value
```

Taken from: https://www.reddit.com/r/javascript/comments/573v6f/is_jquery_still_relevant/d8p2dng
