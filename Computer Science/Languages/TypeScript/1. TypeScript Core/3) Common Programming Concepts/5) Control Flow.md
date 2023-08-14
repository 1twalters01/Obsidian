# Conditions
## If/Else If/Else
Basic `if...else` syntax looks like this:
```
if (condition) {
  /* code to run if condition is true */
} else if {
  /* run some other code instead */
} else {
  /* run 3rd branch instead*/
}
```

### Comparison operators
Comparison operators are used to test the conditions inside our conditional statements. The ones used in JavaScript are as follows:
* `===` and `!==` — test if one value is identical to, or not identical to, another.
* `<` and `>` — test if one value is less than or greater than another.
* `<=` and `>=` — test if one value is less than or equal to, or greater than or equal to, another.

Any value that is not `false`, `undefined`, `null`, `0`, `NaN`, or an empty string (`''`) returns `true` when tested as a conditional statement. Therefore, you can use a variable name on its own to test whether it is `true`, or  exists. So for example:
```javascript
let cheese = "Cheddar";

if (cheese) {
  console.log("Yay! Cheese available for making cheese on toast.");
} else {
  console.log("No cheese on toast for you today.");
}
```

### Logical operators
If you want to test multiple conditions without writing nested `if...else` statements, logical operators can help. When used in conditions, the first two do the following:
* `&&` — AND; allows you to chain together two or more expressions so that all of them have to individually evaluate to `true` for the whole expression to return `true`.
```javascript
if (choice === "sunny" && temperature < 86) {
  para.textContent = `It is ${temperature} degrees outside — nice and sunny. Let's go out to the beach, or the park, and get an ice cream.`;
} else if (choice === "sunny" && temperature >= 86) {
  para.textContent = `It is ${temperature} degrees outside — REALLY HOT! If you want to go outside, make sure to put some sunscreen on.`;
}
```

* `||` — OR; allows you to chain together two or more expressions so that one or more of them have to individually evaluate to `true` for the whole expression to return `true`.
```javascript
if (iceCreamVanOutside || houseStatus === "on fire") {
  console.log("You should leave the house quickly.");
} else {
  console.log("Probably should just stay in then.");
}
```

The last type of logical operator, NOT, expressed by the `!` operator, can be used to negate an expression. Combining it with OR in the above example:
```javascript
if (!(iceCreamVanOutside || houseStatus === "on fire")) {
  console.log("Probably should just stay in then.");
} else {
  console.log("You should leave the house quickly.");
}
```

## Switch Statements
Use a switch statement if you just want to run code based on a variable being a certain choice or condition:
```javascript
switch (expression) {
  case choice1:
    // run this code
    break;

  case choice2:
    // run this code instead
    break;

  // include as many cases as you like

  default:
    // actually, just run this code
    break;
}
```

With an example in use being:
```javascript
const select = document.querySelector("select");
const para = document.querySelector("p");

select.addEventListener("change", setWeather);

function setWeather() {
  const choice = select.value;

  switch (choice) {
    case "sunny":
      para.textContent =
        "It is nice and sunny outside today. Wear shorts! Go to the beach, or the park, and get an ice cream.";
      break;
    case "rainy":
      para.textContent =
        "Rain is falling outside; take a rain coat and an umbrella, and don't stay out for too long.";
      break;
    case "snowing":
      para.textContent =
        "The snow is coming down — it is freezing! Best to stay in with a cup of hot chocolate, or go build a snowman.";
      break;
    case "overcast":
      para.textContent =
        "It isn't raining, but the sky is grey and gloomy; it could turn any minute, so take a rain coat just in case.";
      break;
    default:
      para.textContent = "";
  }
}
```

## Ternary Operator
The ternary or conditional operator is a small bit of syntax that tests a condition and returns one value/expression if it is `true`, and another if it is `false`: 
```javascript
condition ? run this code : run this code instead
```

This can take up a lot less code than an `if...else` block if you have two choices that are chosen between via a `true`/`false` condition.

An example:
```javascript
const greeting = isBirthday
  ? "Happy birthday Mrs. Smith — we hope you have a great day!"
  : "Good morning Mrs. Smith.";
```

# Loops
## For Loop
For Loops have the following syntax:
```javascript
for (initializer; condition; final-expression) {
  // code to run
}
```

As an example:
```javascript
const results = document.querySelector("#results");

function calculate() {
  for (let i = 1; i < 10; i++) {
    const newResult = `${i} x ${i} = ${i * i}`;
    results.textContent += `${newResult}\n`;
  }
  results.textContent += "\nFinished!\n\n";
}

const calculateBtn = document.querySelector("#calculate");
const clearBtn = document.querySelector("#clear");

calculateBtn.addEventListener("click", calculate);
clearBtn.addEventListener("click", () => (results.textContent = ""));
```

### For .. of
The basic tool for looping through a collection is the [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) loop:
```javascript
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (const cat of cats) {
  console.log(cat);
}
```

### exiting loops with break
If you want to exit a loop before all the iterations of a loop have been completed, you can use the [break](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break) statement.

#### example
Say we wanted to search through an array of contacts and telephone numbers and return just the number we wanted to find? First, some simple HTML — a text [`<input>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input) allowing us to enter a name to search for, a [`<button>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) element to submit a search, and a [`<p>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p) element to display the results in:

HTMLPlayCopy to Clipboard

```html
<label for="search">Search by contact name: </label>
<input id="search" type="text" />
<button>Search</button>

<p></p>
```

Now on to the JavaScript:
```javascript
const contacts = [
  "Chris:2232322",
  "Sarah:3453456",
  "Bill:7654322",
  "Mary:9998769",
  "Dianne:9384975",
];
const para = document.querySelector("p");
const input = document.querySelector("input");
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  const searchName = input.value.toLowerCase();
  input.value = "";
  input.focus();
  para.textContent = "";
  for (const contact of contacts) {
    const splitContact = contact.split(":");
    if (splitContact[0].toLowerCase() === searchName) {
      para.textContent = `${splitContact[0]}'s number is ${splitContact[1]}.`;
      break;
    }
  }
  if (para.textContent === "") {
    para.textContent = "Contact not found.";
  }
});
```

### skipping iterations with continue
The [continue](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/continue) statement works similarly to `break`, but instead of breaking out of the loop entirely, it skips to the next iteration of the loop.

Let's look at another example that takes a number as an input, and returns only the numbers that are squares of integers (whole numbers). The HTML is basically the same as the last example — a simple numeric input, and a paragraph for output.
```html
<label for="number">Enter number: </label>
<input id="number" type="number" />
<button>Generate integer squares</button>

<p>Output:</p>
```

The JavaScript is mostly the same too, although the loop itself is a bit different:
```javascript
const para = document.querySelector("p");
const input = document.querySelector("input");
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  para.textContent = "Output: ";
  const num = input.value;
  input.value = "";
  input.focus();
  for (let i = 1; i <= num; i++) {
    let sqRoot = Math.sqrt(i);
    if (Math.floor(sqRoot) !== sqRoot) {
      continue;
    }
    para.textContent += `${i} `;
  }
});
```

## map() and filter()
JavaScript also has more specialized loops for collections, and we'll mention two of them here.

You can use `map()` to do something to each item in a collection and create a new collection containing the changed items:
```javascript
function toUpper(string) {
  return string.toUpperCase();
}

const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const upperCats = cats.map(toUpper);

console.log(upperCats);
// [ "LEOPARD", "SERVAL", "JAGUAR", "TIGER", "CARACAL", "LION" ]
```

You can use [`filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) to test each item in a collection, and create a new collection containing only items that match:
```javascript
function lCat(cat) {
  return cat.startsWith("L");
}

const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const filtered = cats.filter(lCat);

console.log(filtered);
// [ "Leopard", "Lion" ]
```

Using function expressions:
```javascript
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const filtered = cats.filter((cat) => cat.startsWith("L"));
console.log(filtered);
// [ "Leopard", "Lion" ]
```
## while and do...while
`for` is not the only type of loop available in JavaScript. Another one is the `while` loop. This loop's syntax looks like so:
```javascript
initializer
while (condition) {
  // code to run

  final-expression
}
```

Looking at an example:
```javascript
const cats = ["Pete", "Biggles", "Jasmine"];

let myFavoriteCats = "My cats are called ";

let i = 0;

while (i < cats.length) {
  if (i === cats.length - 1) {
    myFavoriteCats += `and ${cats[i]}.`;
  } else {
    myFavoriteCats += `${cats[i]}, `;
  }

  i++;
}

console.log(myFavoriteCats); // "My cats are called Pete, Biggles, and Jasmine."
```

The [do...while](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/do...while) loop is very similar, but provides a variation on the while structure:
```javascript
initializer
do {
  // code to run

  final-expression
} while (condition)
```

The main difference between a `do...while` loop and a `while` loop is that _the code inside a `do...while` loop is always executed at least once_.

Let's rewrite our cat listing example to use a `do...while` loop:
```javascript
const cats = ["Pete", "Biggles", "Jasmine"];

let myFavoriteCats = "My cats are called ";

let i = 0;

do {
  if (i === cats.length - 1) {
    myFavoriteCats += `and ${cats[i]}.`;
  } else {
    myFavoriteCats += `${cats[i]}, `;
  }

  i++;
} while (i < cats.length);

console.log(myFavoriteCats); // "My cats are called Pete, Biggles, and Jasmine."
```