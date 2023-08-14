# Built in Browser Functions
* 
* You can check the full list of the built-in functions, as well as the built-in objects and their corresponding methods [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects).
# Functions
* The built-in code we've made use of so far come in both forms: functions and methods.
	* Functions that are part of objects are called methods.
* You can define functions using the `function` keyword:
```javascript
function draw() {
  ctx.clearRect(0, 0, WIDTH, HEIGHT);
  for (let i = 0; i < 100; i++) {
    ctx.beginPath();
    ctx.fillStyle = "rgba(255,0,0,0.5)";
    ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
    ctx.fill();
  }
}
```

This function draws 100 random circles inside a [`<canvas>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) element. Every time we want to do that, we can just invoke the function with this (remembering the parentheses):
```javascript
draw();
```

You can create functions that require a parameter as follows:
```javascript
function random(number) {
  return Math.floor(Math.random() * number);
}
```

The above function for example gives us a random number between 0 and the number in question.
Use the `return` keyword to return a value from a function.

## Default Parameters
Specify default values by adding `=` after the name of the parameter if you want to support optional parameters:
```javascript
function hello(name = "Chris") {
  console.log(`Hello ${name}!`);
}

hello("Ari"); // Hello Ari!
hello(); // Hello Chris!
```

# Anonymous Functions
So far we have just created a function like so:
```javascript
function myFunction() {
  alert("hello");
}
```

But you can also create a function that doesn't have a name, aka an **anonymous function**:
```javascript
(function () {
  alert("hello");
});
```

* This is called an **anonymous function**, because it has no name.
* You'll often see them when a function expects to receive another function as a parameter.
* In this case, the function parameter is often passed as an anonymous function.

## Example
Instead of defining a separate `logKey()` function, you can pass an anonymous function into `addEventListener()`:
```javascript
function logKey(event) {
  console.log(`You pressed "${event.key}".`);
}

textBox.addEventListener("keydown", logKey);
```

becomes
```javascript
textBox.addEventListener("keydown", function (event) {
  console.log(`You pressed "${event.key}".`);
});
```
## Arrow Functions
If you pass an anonymous function like this, there's an alternative form you can use, called an **arrow function**.

Instead of `function(event)`, you write `(event) =>`:
```javascript
textBox.addEventListener("keydown", (event) => {
  console.log(`You pressed "${event.key}".`);
});
```

If the function only takes one parameter, you can omit the brackets around the parameter:
```javascript
textBox.addEventListener("keydown", event => {
  console.log(`You pressed "${event.key}".`);
});
```

Finally, if your function contains only one line that's a `return` statement, you can also omit the braces and the `return` keyword, and implicitly return the expression.

In the following example we're using the [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) method of `Array` to double every value in the original array:
```javascript
const originals = [1, 2, 3];

const doubled = originals.map(item => item * 2);

console.log(doubled); // [2, 4, 6]
```

The `map()` method takes each item in the array in turn, passing it into the given function. It then takes the value returned by that function and adds it to a new array.

So in the example above, `item => item * 2` is the arrow function equivalent of:
```javascript
function doubleItem(item) {
  return item * 2;
}
```

# Function Scope Conflicts
All scope rule are as expected, other than when using `const`, where you cannot reassign it in another function.