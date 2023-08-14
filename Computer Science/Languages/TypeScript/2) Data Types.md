# Datatypes
JavaScript has 8 Datatypes:
1. String  
2. Number  
3. Bigint  
4. Boolean  
5. Undefined  
6. Null  
7. Symbol  
8. Object

You can use the JavaScript `typeof` operator to find the type of a JavaScript variable. The `typeof` operator returns the type of a variable or an expression:
```javascript
typeof ""             // Returns "string"  
typeof (3 + 4)        // Returns "number"
```

JavaScript has dynamic types. This means that the same variable can be used to hold different data types:
```javascript
let x;       // Now x is undefined  
x = 5;       // Now x is a Number  
x = "John";  // Now x is a String
```

# Strings
When adding a number and a string, JavaScript will treat the number as a string.
```javascript
let x = 16 + 4 + "Volvo";
/* x now equals 20Volvo
```

Strings are written with quotes. You can use single or double quotes:
```javascript
// Using double quotes:  
let carName1 = "Volvo XC60";  
  
// Using single quotes:  
let carName2 = 'Volvo XC60';
```

You can use quotes inside a string, as long as they don't match the quotes surrounding the string:
```javascript
// Single quote inside double quotes:  
let answer1 = "It's alright";    
  
// Double quotes inside single quotes:  
let answer2 = 'He is called "Johnny"';
```

## Numbers
All JavaScript numbers are stored as decimal numbers (floating point). Numbers can be written with, or without decimals:
```javascript
// With decimals:  
let x1 = 34.00;  
  
// Without decimals:  
let x2 = 34;
```

Extra large or extra small numbers can be written with scientific (exponential) notation:
```javascript
let y = 123e5;    // 12300000  
let z = 123e-5;   // 0.00123
```

All JavaScript numbers are stored in a a 64-bit floating-point format.

JavaScript `BigInt` is a new datatype ([ES2020](https://www.w3schools.com/js/js_2020.asp)) that can be used to store integer values that are too big to be represented by a normal JavaScript Number, for example:
```javascript
let x = BigInt("123456789012345678901234567890");
```
https://www.w3schools.com/js/js_bigint.asp

## Booleans
Booleans can only have two values: `true` or `false`.

# Arrays and Objects
JavaScript arrays are written with square brackets and separated by commas:
```javascript
const cars = ["Saab", "Volvo", "BMW"];
```

JavaScript objects are written with curly braces `{}`. Object properties are written as name:value pairs, separated by commas.
```javascript
const person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
```

The object data type can contain:
* An object  
* An array  
* A date

# Empty and Undefined
In JavaScript, a variable without a value, has the value `undefined`. The type is also `undefined`.
```javascript
let car;    // Value is undefined, type is undefined
```

Any variable can be emptied, by setting the value to `undefined`. The type will also be `undefined`.
```javascript
car = undefined;    // Value is undefined, type is undefined
```

An empty value has nothing to do with `undefined`. An empty string has both a legal value and a type:
```javascript
let car = "";    // The value is "", the typeof is "string"
```