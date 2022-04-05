---
layout: post
title: Class 2
---

####`authenticity_token`
To prevent CSRF (Cross-Site Request Forgery)
```
<%= form_with %>

<form method="post"> 
  <input
    type="hidden" 
    name="authenticity_token" 
    value="RANDOM_STRING"
</form>
```

####`<%= link_to %>` and `<%= form_with %>`
- We'll never write an `<a>` tag again to link two webpages (e.g. to point back to previous page). Instead, we'll use `<%= link_to %>`, for example:
  ```
  <%= link_to "Go back", "/movies" %>
  ```

####`.where` vs. `.find_by` vs. `.find` to query a `Hash`:
- In App Dev 1, we often used `Hash.where({}).at(0)` to return the first object because `.where` returns a relation and `.at(0)` returns first record within that relation. Now we can just use `.find_by` to achieve the same thing.
- `.where` will always returns a relation whether it's empty or not, whereas `.find_by` will either return a record or `nil`.
- `.find` can only be used with ID
