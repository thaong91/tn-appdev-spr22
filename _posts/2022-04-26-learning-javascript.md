---
layout: post
title: Learning Javascript
---

### Variables
---
#### When to use `const` vs. `let`
`const` is more frequently used for declarative variables (like numbers or boolean) because it has many restrictions that make code more readable, including:
1. It must be initialized with value
2. Can't be reassigned after declaration

#### Variable shawdowing:
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
---
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
    We need to pay attention to operator precendence, where `&&` is executed before `||` and parenthesis `()` has the highest precedence.

### Functions
---
- Declare a function with `function` followed by its name, arguments within `()` and then specify the function within `{}`.
- Variables declared within a function will only be valid within that function ("function-scoped" and not global scope). Conversely, functions can access any global-scoped variables.
- Instead of throwing errors, Javascript will just ignore any additional arguments that were passed into a function if they were not declared previously. 
- To return a value from a function, use `return` keyword.
- An example function:
  ```javascript
  function splitBill(amount, numPeople) {
    return `Each person pays ${amount / numPeople}`
  } 
  ```
- Functions can be returned (just like variables) within other functions. For example:
  ```javascript
  function handleLikePost(step) {
    let likeCount = 0;
    return function addLike() {
      likeCount += step;    
      return likeCount;
    }
  }
  const like = handleLikePost(1);
  console.log(like());
  ```
- Closures are a property of Javascript functions and not any other languages. We can observe closures by calling function in a different scope than where function was original defined. In the example above, JS is actually accessing `likeCount` variable here without us having to call `addLike()` function, because closure preserves this variable instead of destroying it after executing the `handleLikePost` function.
- Specifying default value of a variable within the function argument using `=`, for example:
  ```javascript
  function convertTemperature(celsius, decimalPlaces = 1)
  ```
#### Arrow  Functions: `=>`
- Instead of writing:
  ```javascript
  function capitalizeName(name) {
    return `${name.charAt(0).toUpperCase()}${name.slice(1)}`;  
  }
  ```
  we can re-write this function using arrow functions:
  ```javascript
  const capitalize = (name) => {
    return `${name.charAt(0).toUpperCase()}${name.slice(1)}`;
  }
  ```
- We can actually drop the `()` around `(name)` and just write `name`, since there is only one variable here. But we'll need to use `()` if there are more than one variable.
- We can also remove the `{}` and `return` keywords altogether to make the function more concise:
    ```javascript
    const capitalize = name => `${name.charAt(0).toUpperCase()}${name.slice(1)}`;
    ```
- Callback function is just a function that is called after another function and is a high-order function. For example:
    ```javascript
    const username = 'john';
    const capitalize = name => `${name.charAt(0).toUpperCase()}${name.slice(1)}`;
    
    function greetUser(name, callback) {
      return callback(capitalize(name));
    }
    
    const result = greetUser(username, name => `Hi there, ${name}!`);
    ```
    In this case `callback` function will be called/executed after `capitalize()` function has been called. If we `console.log(result)` in this case, we'll get `Hi there, John!` as result.
- With that, we can re-write the `splitBill` function earlier to arrow function:
    ```javascript
    const splitBill = (amount, numPeople) => `Each person needs to pay ${amount / numPeople}`
    ```
- Also, re-writing the `coundDown` function to:
    ```javascript
    const countdown = (startingNumber, step) => {
      let countFromNum = startingNumber + step;
      return () => countFromNum -= step;
    }
    ```
- Partial application is particularly useful to separate functions out to more distinctive roles and increase re-usability.
    ```javascript
    function getData(baseUrl) {
       return function(route) {
         return function(callback) {
           fetch(`${baseUrl}${route}`)
             .then(response => response.json())
             .then(data => callback(data));  
         }
       }
    }

    const getSocialMediaData = getData('https://jsonplaceholder.typicode.com');

    const getSocialMediaPosts = getSocialMediaData('/posts');
    const getSocialMediaComments = getSocialMediaData('/comments');
    
    getSocialMediaPosts(posts => {
    posts.forEach(post => console.log(post.title));
    });
    ```
- Function should be named with action verbs

### Objects and Maps
---
#### Objects
- Objects are used to organized related and unchanging data. 
    ```javascript
    const colors = {blue:'#f00', orange: '#f60', red: '#00f'}
    ```
- Objects are reference type and not primitive type
- We can access the value of the above object with `color[blue]`
- We can destructure object and make certain keys to become variables by:
    ```javascript
    const { blue, orange } = myColor;
    ```
    Another example:
    ```javascript
    const user = {
      name: "Reed",
      username: "Reedbarger",
      email: "reed@gmail.com",
      details: {
        title: "Programmer"
      }
    };
    
    const { name, details: { title} } = user;
    
    function displayUserBio() {
      console.log(`${name} is a ${title}`);
    }
    ```
    After that, both `name` and `title` are now variables that we can accessed more easily. If we run `displayUserBio()`, we'll get `Reed is a Programmer`
- To use this function to access data of multiple users with similar `user` object structure, we can also rewrite the `displayUserBio` function to:
  ```
  function displayUserBio({ name, details: { title} }) {
    console.log(`${name} is a ${title}`);
  }
  ```
- If we want to create new object with similar structure to `user` above, but don't have all the information available, we can:
  ```javascript
  const newUser = {
  username: "ReedBarger",
  email: "reed@gmail.com",
  password: "mypassword"  
  };

  Object.assign({}, user, newUser);
  ```
  the `{}` is to create a new empty object that is the same as `user` and ensure that we don't mutate values of the original `user`. 
  However, we can tell JS explicitly to create a new Object merging `user` and `newUser` and spreading all properties of existing Objects (using `...` syntax):
  ```javascript
  const createdUser = { ...user, ...newUser, verified: false };
  ```
- If Object key (which we can get with `Object.keys()`) is not a string, then JS will automatically convert it into string.

#### Maps
- Create new Map:
  ```javascript
  new Map([
    ['key', 'value']  
  ]);
  ```
- `.set('key', 'value')` method mutates a Map. If Object is unordered, Map is ordered and so it will keep the same order of all key-value pairs.
- `.forEach()` is an iterative method for Map. For example:
  ```javascript
  const map1 = new Map([
    [1, 1],
    [true, true]  
  ]);
  
  map1.forEach((value, key) => {
    console.log(key, value);  
  });
  ```
- `.get(key)` to get a value from key-value pair in a Map.
- `.size()` is used to get the number of key-value pairs in a Map.
- `WeakMap` can be useful to create Map where key is another Object so that we can easily throw away when done and save memory.
- `this` keyword used to extract value from a Map, for example:
  ```javascript
  const userData = { 
    username: "Reed",
    title: "JavaScript Programmer",
    getBio() {
      console.log(`User ${this.username} is a ${this.title}`);
    }  
  }
  ```
  `userData.getBio()` will get the right values.
- Using arrow function, we can also access `this` in another nested function:
  ```javascript
    const userData = { 
    username: "Reed",
    title: "JavaScript Programmer",
    getBio() {
      console.log(`User ${this.username} is a ${this.title}`);
    },
    askToFriend() {
    setTimeout(() => {
      console.log(`Would you like to friend ${this.username}?`);   
    }, 2000
  }
  );  
  ```
