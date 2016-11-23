# A tweet-sized JavaScript templating engine

```js
function t(s,d){
 for(var p in d)
   s=s.replace(new RegExp('{'+p+'}','g'), d[p]);
 return s;
}

//Call it like this:
//view sourceprint?
t("Hello {who}!", { who: "JavaScript" });
// "Hello JavaScript!"

t("Hello {who}! It's {time} ms since epoch.", { who: "JavaScript", time: Date.now });
// "Hello JavaScript! It's 1299680443046 ms since epoch."
```

Take from: http://mir.aculo.us/2011/03/09/little-helpers-a-tweet-sized-javascript-templating-engine/
