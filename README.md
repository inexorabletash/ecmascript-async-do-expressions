# Asynchronous "do expressions"

## Overview

Building on
[async functions](https://github.com/tc39/ecmascript-asyncawait)
and the not-formally-proposed 
["do expressions"](http://wiki.ecmascript.org/doku.php?id=strawman:do_expressions),
"async do expressions" let you write asynchronous blocks in an
expression context, returning the completion value (a Promise).

This is useful when functions expect an asynchronous result (a Promise)
as input, and the steps to produce that result can be expressed in line
with the function call.

Example:
```js
fetchEvent.respondWith(async {
  var response = await fetch('https://example.com/resource');
  var body = await response.text();
  'BEGIN---\n' + body + '\n---END\n';
});
```

This is similar to an "immediately invoked async function expression", i.e.:

```js
fetchEvent.respondWith((async () => {
  var response = await fetch('https://example.com/resource');
  var body = await response.text();
  return 'BEGIN---\n' + body + '\n---END\n';
})());
```

Or more verbosely:

```js
async function f() {
  var response = await fetch('https://example.com/resource');
  var body = await response.text();
  return 'BEGIN---\n' + body + '\n---END\n';
};
let p = f();
fetchEvent.respondWith(p);
```

## Syntax

```
PrimaryExpression :
  ...
  AsyncBlockExpression
  
AsyncBlockExpression :
  "async" [no LineTerminator here] Block
  
ExpressionStatement :
  [lookahead ∉ {{, function, class, let [, async}] Expression;
```

## Semantics

TODO - static and runtime semantics

* `var` hoisting behavior needs definition
* `break`, `continue` and `return` need definition

## References

* [async functions](https://github.com/tc39/ecmascript-asyncawait)
* [do expressions strawman](http://wiki.ecmascript.org/doku.php?id=strawman:do_expressions)
* TC39 meeting notes: [Do Expression](https://github.com/rwaldron/tc39-notes/blob/master/es6/2014-01/jan-29.md#do-expression)
* es-discuss thread: [monadic extension to do-notation](https://esdiscuss.org/topic/monadic-extension-to-do-notation)

