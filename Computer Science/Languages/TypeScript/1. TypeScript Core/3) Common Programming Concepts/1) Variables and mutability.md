# Variables
* You create a variable with the keyword `let` followed by a name for your variable.
	* These are mutable, have block scope, and cannot be redeclared.

```typescript
let x: number = 5;
x = x + 3; // correct
let x = 8; // error

```

Variables declared inside a { } block cannot be accessed from outside the block:
```typescript
{  
  let x = 2;  
}  
// x can NOT be used here
```

Redeclaring a variable inside a block will not redeclare the variable outside the block:
```typescript
let x = 10;  
// Here x is 10  
  
{  
  let x = 2;  
  // Here x is 2  
}  
  
// Here x is 10
```

# Constants
* You create a constant with the keyword const.
	* JavaScript `const` variables must be assigned a value when they are declared:

```typescript
// Incorrect
const PI
PI = 3.14159265359;
```

```TypeScript
// Correct
const PI = 3.14159265359;
PI = PI + 3               // Gives an error
```

Always declare a variable with `const` when you know that the value should not be changed.

* The keyword `const` is a little misleading. It does not define a constant value. It defines a constant reference to a value.
* Because of this you can NOT:
	* Reassign a constant value
	* Reassign a constant array
	* Reassign a constant object
* But you can:
	* Change the elements of constant array
	* Change the properties of constant object

```typescript
// You can create a constant array:  
const cars = ["Saab", "Volvo", "BMW"];  
  
// You can change an element:  
cars[0] = "Toyota";  
  
// You can add an element:  
cars.push("Audi");
```

But you can NOT reassign the array:
```typescript
const cars = ["Saab", "Volvo", "BMW"];
cars = ["Toyota", "Volvo", "Audi"];    // ERROR
```

## Block Scope
Declaring a variable with `const` is similar to `let` when it comes to **Block Scope**.

The x declared in the block, in this example, is not the same as the x declared outside the block:
```typescript
const x = 10;  
// Here x is 10  
  
{  
  const x = 2;  
  // Here x is 2  
}  
  
// Here x is 10
```

Redeclaring an existing `var` or `let` variable to `const`, in the same scope, is not allowed:
```typescript
var x = 2;     // Allowed  
const x = 2;   // Not allowed  
  
{  
  let x = 2;     // Allowed  
  const x = 2;   // Not allowed  
}  
  
{  
  const x = 2;   // Allowed  
  const x = 2;   // Not allowed  
}
```