* An object is a collection of related data and/or functionality.
* These usually consist of several variables and functions (which are called properties and methods when they are inside objects).
* Creating an object often begins with defining and initializing a variable.

Most things in JavaScript are objects. `split` is a method available on  [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) object, for example.

# Object Basics
The following is an empty object:
```javascript
const person = {};
```

This is an object with properties and a method:
```javascript
const person = {
  name: ["Bob", "Smith"],
  age: 32,
  bio: function () {
    console.log(`${this.name[0]} ${this.name[1]} is ${this.age} years old.`);
  },
  introduceSelf: function () {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```

We can write methods in a shorter way:
```javascript
const person = {
  name: ["Bob", "Smith"],
  age: 32,
  bio() {
    console.log(`${this.name[0]} ${this.name[1]} is ${this.age} years old.`);
  },
  introduceSelf() {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```

An object like this is referred to as an **object literal** — we've literally written out the object contents as we've come to create it. This is different compared to objects instantiated from classes.

# Dot Notation
You can access the object's properties and methods using **dot notation**. The object name (person) acts as the **namespace**. You then write a `.` and then what you want to access:
```javascript
person.age;
person.bio();
```

## Objects as Object properties
An object can be an object property. For example:
```javascript
const person = {
  name: {
    first: "Bob",
    last: "Smith",
  },
  // …
};
```

To access these items you would do the following:
```javascript
person.name.first;
person.name.last;
```

If you do this, you'll also need to go through your method code and change any instances of
```javascript
name[0];
name[1];
```

to
```javascript
name.first;
name.last;
```

# Bracket Notation
Instead of doing dot notation
```javascript
person.age;
person.name.first;
```

you can do bracket notation
```javascript
person["age"];
person["name"]["first"];
```

This looks similar to how you access values in an array. In fact, objects are sometimes called **associative arrays** — they map strings to values in the same way that arrays map numbers to values.

Dot notation is generally preferred over bracket notation because it is more succinct and easier to read. However there are some cases where you have to use brackets.
For example, if an object property name is held in a variable, then you can't use dot notation to access the value, but you can access the value using bracket notation.

For example:
```javascript
const person = {
  name: ["Bob", "Smith"],
  age: 32,
};

function logProperty(propertyName) {
  console.log(person[propertyName]);
}

logProperty("name");
// ["Bob", "Smith"]
logProperty("age");
// 32
```

# Setting Object Members
you can set the value of object members by declaring the member you want to set (using dot or bracket notation), like this:
```javascript
person.age = 45;
person["name"]["last"] = "Cratchit";
```

You can also create completely new members:
```javascript
person["eyes"] = "hazel";
person.farewell = function () {
  console.log("Bye everybody!");
};
```

Bracket notation can be used to set member names.
```
const myDataName = "height"
const myDataValue = "1.75m";

person[myDataName] = myDataValue;

console.log(person.height)
```

# This
Look at this:
```javascript
introduceSelf() {
  console.log(`Hi! I'm ${this.name[0]}.`);
}
```

The `this` keyword refers to the current object the code is about. This is not useful when you have one object, as we could just write `person.name`, but is useful when you have many.

Consider
```javascript
const person1 = {
  name: "Chris",
  introduceSelf() {
    console.log(`Hi! I'm ${this.name}.`);
  },
};

const person2 = {
  name: "Deepti",
  introduceSelf() {
    console.log(`Hi! I'm ${this.name}.`);
  },
};
```

Instead of writing `person1.name` and `person2.name` inside of `introduceSelf`, we can just write the same code.

# Constructor Introduction
Using object literals is fine when you only need to create one object, but if you have to create more than one, as in the previous section, they lead to a lot of duplication.

We can do this:
```javascript
function createPerson(name) {
  const obj = {};
  obj.name = name;
  obj.introduceSelf = function () {
    console.log(`Hi! I'm ${this.name}.`);
  };
  return obj;
}

const salva = createPerson("Salva");
salva.introduceSelf();
```

but it is longwinded.

Constructors, by convention, start with a capital letter and are named for the type of object they create. So we could rewrite our example like this:
```javascript
function Person(name) {
  this.name = name;
  this.introduceSelf = function () {
    console.log(`Hi! I'm ${this.name}.`);
  };
}
```

To call `Person()` as a constructor, we use `new`:
```javascript
const salva = new Person("Salva");
salva.name;
salva.introduceSelf();
// "Hi! I'm Salva."
```