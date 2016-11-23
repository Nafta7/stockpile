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
