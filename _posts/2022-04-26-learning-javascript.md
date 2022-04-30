---
layout: post
title: Learning Javascript
---

### Variables
--
#### When to use `const` vs. `let`
`const` is more frequently used for declarative variables (like numbers or boolean) because it has many restrictions that make code more readable, including:
1. It must be initialized with value
2. Can't be reassigned after declaration

##### Variable shawdowing:
```javascript
  var price = 20;
  var isSale = true;

  // variable shadowing
  if (isSale) {
    // discount price of product
    var price = 20 - 2; 
    // do something 
    console.log('sale price', price);
  }

  console.log('price', price);
```
This block of code will print out `18` instead of `20` because it ignores the code block didn't create a new variable `price` but rather updating value of the global `price` variable that we defined using `var`.
To fix this issue, we use `let` and `const` instead, as they are "block-scoped`:
```javascript
  if (isSale) {
    // discount price of product
    let price = 20 - 2; 
    // do something 
    console.log('sale price', price);
  }
```
#### Template literals
Using `${}` to wrap around variables and then wrap the string with ` `` ` will make code simpler and more printable. For example:
```javascript
  let username = "Jane Doe";
  let message = `Hi ${username}, how are you?`;
  console.log(message);
```
To have a string that span multiple lines, we use `\n` to do so.

#### So, how should variables be named?
- Variable names are case-sensitive
- Variable names should be descriptive so that it's easier to understand
- camelCase syntax
- Only letters, numbers, and `$` and `_` symbols are allowed
- Boolean should have `is` or `has` at the beginning of the variable name
- To signify variable names should not be changed by other developers, we write it in all caps like `COLOR_RED`

### Types & Conditionals
--
#### `If` and `Switch` statements:
`If`, `else if `, and `else` statement in javascript is very similar to other languages (with the exception of syntax). For example:
```javascript
  const colorMode = 'dark';

  if (colorMode === 'solarized') {
     console.log('solarized mode set!'); 
     document.body.style.background = 'palegoldenrod';
  } else if (colorMode === 'dark') {
    console.log('dark mode set!');  
    document.body.style.background = 'black';
  } else {    
    console.log('light mode set!');
    document.body.style.background = 'ghostwhite';
  }
```
Note that we use `===` (instead of `==` like in Ruby and Python) to check a condition. But since we don't want to write those cases many times, we can use `switch`, `case`, and `default`:
```javascript
const colorMode = 'dark';

switch (colorMode) {
  case "solarized":
   console.log('solarized mode set!'); 
   document.body.style.background = 'palegoldenrod';
  case 'dark':
    console.log('dark mode set!');  
    document.body.style.background = 'black';
    break;
  default:  
    console.log('light mode set!');
    document.body.style.background = 'ghostwhite';
}
```
`break` is used here to tell the program to stop running any code below that line if the conditions above it have been met.

#### Types
- We can convert one type of variable to another. 
    - Explicit type conversion
    ```javascript
    let message = 'some string';
    
    console.log(String(message));
    ```
    - Implicit(automatic or coercion) type conversion:
    ```javascript
      console.log('1' * '2');
    ```
    Javascript will automatically convert those string variables to numbers because `*` is not a string operator.
- The following six values are default to "falsy" in Javascript: `false`, `0`, `''`, `null`, `undefined`, `NaN`.
- How to avoid confusion when dealing with falsy and truthy values:
    1. Avoid direct comparisons in conditionals. For example:
        ```javascript
        const username = null;
        if (!username) {
        console.log('no user');
        }
        ```
    2. Use triple equals === (strict equals operator). For example:
        ```javascript
        if (null === undefined) {
        console.log('equals');
        } else {
        console.log('not equals');
        }
        ```
    
    3. Convert to real Boolean values where needed
        ```javascript
        if (Boolean(NaN) === Boolean(NaN)) {
        console.log('equal');
        } else {
        console.log('not equals');
        }
        ```
#### Terneries and Short-circuiting:
- **Terneries** are helpful to shorten conditional statements where variables are conditionally assigned. For example, this `if` statement:
    ```javascript
    const isAuthenticated = false;
    let cartItemCount = 0;
    
    if (isAuthenticated) {
        // add item to cart
        cartItemCount = 1;
    } else {
        cartItemCount = 0;
    }
    ```
    can be rewritten as:
    ```javascript
    const cartItemCount = isAuthenticated ? 1 : 0;
    ```
- If there are multiple `else if` conditions, we can technically chain terneries together. However, this will make our code more difficult to read, so it's not recommended.
- **Short-circuiting** is antoher way to shorten conditionals even more by using operators `||` (OR) and `&&` (AND) when it evaluates an argument and return either `true` value or a fallback or default value. For example, the following code:
    ```javascript
    const username;
    const isEmailVerified = true;

    if (response) {
        if (isEmailVerified) {
            username = response;
        }
    } else {
        username = "guest";
    }
    ```
    can be re-written as:
    ```javascript
    const username = isEmailVerified && response || "guest";
    ```
    We need to pay attention to operator precendence, where `&&` is executed before `||`.
