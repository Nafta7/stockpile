# ES2015 gist

## Destructuring

ES5:
```js
var person = {
  name: "Gumbo",
  title: "Developer",
  data: "yes"
}

function display(person) {
  var name = person.name
  var title = person.title
  var data = person.data

  console.log(name + ' ' + title + ' ' + data)
}

display(person)
```

ES2015:
```js
var person = {
  name: "Gumbo",
  title: "Developer",
  data: "yes"
}

function display({ name, title, data }) {
  console.log(`${name} ${title} ${data}`)
}

display(person)
```

## Rest/Spread operators

### Rest - collect arguments into an array

```js
function concat(joiner, ...args) {

  // args is an actual Array

  return args.join(joiner)

}

concat(‘_’, 1, 2, 3) // returns ‘1_2_3’
```

### Spread -  expand an array into parameters

```js
const numbersArray = [1, 2, 3]
coolFunction(...numbersArray)

// same as
coolFunction(1, 2, 3)
```
