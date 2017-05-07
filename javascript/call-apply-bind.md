# Difference between call and apply

The difference is that apply lets you invoke the function with arguments as an array;
call requires the parameters be listed explicitly.
A useful **mnemonic** is `A` for `Array` and `C` for `Comma`.

See [here](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Function/apply) and
[here](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Function/call).

Pseudo syntax:
```js
theFunction.apply(valueForThis, arrayOfArgs)

theFunction.call(valueForThis, arg1, arg2, ...)
```

Sample code:

```js
function theFunction(name, profession) {
    alert("My name is " + name + " and I am a " + profession + ".");
}
theFunction("John", "fireman");
theFunction.apply(undefined, ["Susan", "school teacher"]);
theFunction.call(undefined, "Claude", "mathematician");
```

Taken from: http://stackoverflow.com/a/1986909


# Promises, pass additional parameters to then chain

You can use Function.prototype.bind to create a new function with a value passed to its first argument, like this:

```js
P.then(doWork.bind(null, 'text'))

function doWork(text, data) {
  consoleToLog(data);
}
```

Now, text will be actually 'text' in doWork and data will be the value resolved by the Promise.

Source: http://stackoverflow.com/a/32912570/6598709
