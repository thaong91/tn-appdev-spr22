---
layout: post
title: App Dev 2 is off to a great start
---

Here are (at least) three things I learned from [The Ruby Koans](http://rubykoans.com/) and the first App Dev 2 class:
- How to createe a Github Pages page
- Markdown is cool
- I can direcly edit files on github.com
- Git is still confusing


## Notes on Rails:


### get/post/patch/delete

- In HTML, there are only two methods: `get` and `post`. 
- In Rails, there are also `patch` and `delete`, which give us more robust options to update (or "patch") or delete a record. To "fake" these two methods in HTML, we use `data-method:` instead of `method:`


### Shortcuts:
- If a template filename matches an `action` in `controlller`, we can skip the whole `render({ :template => "")` row and Rails will still know which template to render.
- There are seven RESTful routes for a Rails application, adhering to [this naming convention](https://restfulapi.net/resource-naming/). Typically (and originally in App dev 1), we wrote all these routes in `routes.db` file, but we can use the shortcut `resources(:model)`, e.g. `resources(:directors` or `resources(:movies)` and this will automatically create the seven routes.

### Splat `*` operator
- Splat operator `*` is another aspect that is rather new and ... confusing to me in Ruby. First of all, splat can be used to construct an Array. Then it's can also be used to desconstruct Arrays, for example:
- Constructing Array:
  ```
  > numbers = *123
  > numbers
  # => *[123]*
  ```
- Deconstructing Array:
  ```
  > first_name, *last_name = ["John", "Smith", "III"]
  > p last_name
  # => _["Smith", "III"]_
  ```

### 


### Others:
- `/rails/info/routes`: contains list of all routes
- In Ruby, both `0` and `1` are `true` (whereas in other languages, such as Python, `0` means `false`). Only `false` and `nil` mean `false` in Ruby. All non-`false` and non-`nil` objects are considered `true` in Ruby. 
- `.inspect` method is a string class method that returns a printable version of a given string, surrounded by quotation marks, with special characters escaped, for example:
  ```
  > 123.inspect
  # => _"123"_
  
  > nil.inspect
  # => _"nil"_
  ```
- `.object_id` method is a random identifier for an object.
- `.clone` method creates a copy of an object but keeps the original object. The two objects, original and clone, are different objects with different `object_id`
- Unlike `NULL` in other programming languages, `nil` in Ruby is also an object.
- 

## Some notes on App Dev 2 class:
- Instead of using `appdev-projects/base-rails` repo to create a blank new rails app, we'll now use `appdev-project/vanilla-rails` to create more robust Rails applications from now on.

