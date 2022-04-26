---
layout: post
title: Learning Javascript
---

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


