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

### Arrays and Sets
---
#### Arrays
- Unlike Object, whose elements are key-value pairs and are identifiable by its key and position agnostic, Array keeps the order in which element is added.

##### Some useful array methods:
- `Array.push()` to add an element/item to an array
- `Array.pop()` to remove the last element of an array
- `Array.indexOf()` to find index of an element in an array. It will return the value `-1` if it cannot find an element. `Array.findIndex()` is similar.
- `Array.includes()` return a boolean on whether an element (string, integer, or boolean) exist in an array. We use `Array.some()`, which also returns a boolean, instead when Array contains Objects, for example:
  
  ```javascript
  const temperatures = [
    { degrees: 69, isRecordTemp: false }, 
    { degrees: 82, isRecordTemp: true }, 
    { degrees: 73, isRecordTemp: false }, 
    { degrees: 64, isRecordTemp: false }
  ];

  temperatures.some(temperature => temperature.isRecordTemp === true);
  ```
  `Array.some()` will return `true` if at least one of the item meets the condition whereas `.every()` will iterate through all items and will only return `true` if all of them meet the condition.
- `Array.map()` can iterate through and perform actions on all elements of the array and then return a new array. For example:
  
  ```javascript
  const newTemps = temperatures.map(temperature => 
  temperature.degrees > 70 ? { ...temperature, isHigh: true } : temperature);
  ```
- `Array.forEach()` also iterate through all elements in an array, but it can perform an action without returning a new array. 
  
  ```javascript
  newTemps.forEach(temperature => {
   if (temperature.isHigh) {
     console.log(`Temperature ${temperature.degrees} was a record high last week!`);  
   }
  ```
- `Array.filter()` is used to get a subset of array if it's satisfied a condition:

  ```javascript
  const restaurants = [
  { name: 'Cap City Diner', milesAway: 2.2 },
  { name: 'Chop Shop', milesAway: 4.1 },
  { name: 'Northstar Cafe', milesAway: 0.9 },
  { name: 'City Tavern', milesAway: 0.5 },
  { name: 'Shake Shack', milesAway: 5.3 }
  ]

  const results = restaurants.filter(restaurant => restaurant.name.startsWith('C'))
  ```
  it will return an empty array `[]` if no element meets searching requirement.
- `Array.find()` works like `.filter()` but will only return the first element that is a match
- `Array.reduce()`also iterates through all elements, but we'll need to pass in: a callback function and an initial value. For example:
  ```javascript
  const menuItems = [
  { item: "Blue Cheese Salad", price: 8 },
  { item: "Spicy Chicken Rigatoni", price: 18 },
  { item: "Ponzu Glazed Salmon", price: 23 },
  { item: "Philly Cheese Steak", price: 13 },
  { item: "Baked Italian Chicken Sub", price: 12 },
  { item: "Pan Seared Ribeye", price: 31 }
  ];

  const total = menuItems.reduce((accumulator, menuItem) => {
    return accumulator + menuItem.price;  
  }, 0);
  ```
  Starting at `0`, the callback function will take `price` from each element of `menuItems` and add them up and store that value in the `accumulator` variable.
- `.push()` does mutate an array whereas `.concat()` adds new item to a copy of an existing array while not mutae the original array. Similarly, we can use the spread operator `...` to create a copy of an array. For example:

  ```javascript
  const lunchMenuIdeas = ['Harvest Salad', 'Southern Fried Chicken'];
  const allMenuIdeas = lunchMenuIdeas;
  allMenuIdeas.push('Club Sandwich');
  ```
  
  The above code will resolve in `lunchMenuIdeas` being mutated, whereas this will not:
  
  ```javascript
  const lunchMenuIdeas = ['Harvest Salad', 'Southern Fried Chicken'];
  const allMenuIdeas = [...lunchMenuIdeas];
  allMenuIdeas.push('Club Sandwich');
  ```
- `Array.slice(starting_index, ending_index)` is used to get a subset of an array without mutating it
- Destructure an array is similar to that of an object:
  
  ```javascript
  const finalMenuItems = [
  "American Cheeseburger",
  "Southern Fried Chicken",
  "Glazed Salmon"
  ];

  const [first, second, third] = finalMenuItems;
  ```
  
  Additionally, if we want to store the first element as `winner` and the rest of the elements in a new array called `losers`, we'll the rest operator `...` (which is the same as the spread operator, so it's slightly confusing):
  
  ```javascript
  const [winner, ...losers] = finalMenuItems;
  ```
##### Converting Objects into flexible Arrays:
- `Object.keys()` is a method to iterate over all keys in an object. We can chain other array methods after that. For example:
  
  ```javascript
  const user = {
  name: 'John',
  age: 29  
  };
  
  const ageExists = Object.keys(user).includes("age");
  ```
- `Object.values()` is a method that iterates over all values:
  
  ```javascript
  const monthlyExpenses = {
  food: 400,
  rent: 1700,
  insurance: 550,
  internet: 49,
  phone: 95  
  };

  const monthlyTotal = Object.values(monthlyExpenses).reduce((acc, expense) => acc + expense, 0);
  console.log(monthlyTotal);
  ```
- `Object.entries()` is used to iterate over all elements in an Object, both keys and values:
  ```javascript
  const users = {
  '2345234': {
    name: "John",
    age: 29
  },
  '8798129': {
    name: "Jane",
    age: 42
  },
  '1092384': {
    name: "Fred",
    age: 17 
  }
  };

  const usersOver20 = Object.entries(users).reduce((acc, [id, user]) => {
    if (user.age > 20) {
      acc.push({ ...user, id });
    }  
    return acc;
  }, []);
  
  console.log(usersOver20);
  ```
  `[id, user]` above is array destructuring where we save all object keys into `id` and the name and age into `user`. 
 
 #### Sets
 - Set is a new data structure that was added to ES6
 - Set tosses out any repetitive item.
 - Create a new set by `new Set()`
 
   ```javascript
   const customerDishes = [
    "Chicken Wings",
    "Fish Sandwich",
    "Beef Stroganoff",
    "Grilled Cheese",
    "Blue Cheese Salad",
    "Chicken Wings",
    "Reuben Sandwich",
    "Grilled Cheese",
    "Fish Sandwich",
    "Chicken Pot Pie",
    "Fish Sandwich",
    "Beef Stroganoff"
  ];

  const uniqueDishes = [...new Set(customerDishes)];
  console.log(uniqueDishes)
  ```
  
