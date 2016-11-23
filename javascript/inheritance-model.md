# Inheritance Model

## 1. NATIVE/PSEUDO-CLASSICAL

```js
// Constructor function
var Person = new function(name, gender){
    // Instance level variable
    this.name = name
    this.gender = gender
}
```

```js
// Inherited properties
Person.prototype.sayHi = function(){
    alert("Hi I'm " + this.name + ", and I'm a" this.gender)
}

var shepard = new Person("Shepard", "robot")
shepard.sayHi()
```


## 2. CLASSICAL INHERITANCE

```js
//  Base class
var Person = Class.extend({
    // Constructor
    init: function(name, gender){
        this.name   = name
        this.gender = gender
   },
   sayHi: function(){
       alert("I'm person: " + this.name)
   }
})

// Inherited class
var Javascripter = Person.extend({
    init: function(name, gender){
        // Call to parent
        this._super(name, gender)
        this.hatesJava = true
    },
    // Function override
    sayHi: function(){
        alert("OMG I'm: " + this.name)
    }
})

var shepard = new Javascripter("Shepard", "Megaman")
shepard istanceof Person //true
shepard instanceof Javascripter //truer
```

## 3. PROTOTYPAL INHERITANCE

if (typeof Object.create !== "function"){
    Object.create = function(o){
        var F = function(){}; // rofl jQuery.noop()
        F.prototype = o;
        return new F();
    };
}

// Object literal means a usable person
var actualPerson = {
    name: "JohnDoe",
    gender: "Unisex",
    sayHi: function(){
        alert(this.name + " is a " + this.gender);
    }
};

// make a clone
var shepard = Object.create(actualPerson);
// then describe the differences
shepard.name = "Shepard";
shepard.gender = "Superman";

Taken from:
http://www.slideshare.net/SlexAxton/how-to-manage-large-jquery-apps
