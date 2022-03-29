---
layout: post
title: Three things I learned from The Ruby Koans
---

Here are (at least) three things I learned from [The Ruby Koans](http://rubykoans.com/):
- How to createe a Github Pages page
- Markdown is cool
- I can direcly edit files on github.com
- Git is still confusing


## Notes on Rails:


### get/post/patch/delete

- In HTML, there are only two methods: `get` and `post`. 
- In Rails, there are also `patch` and `delete`, which give us more robust options to update (or "patch") or delete a record. To "fake" these two methods in HTML, we use `data-method:` instead of `method:`


### Shortcuts:
- If a template filename matches an `action` in `controlller` 
- There are seven RESTful routes for a Rails application, adhering to [this naming convention](https://restfulapi.net/resource-naming/). Typically (and originally in App dev 1), we wrote all these routes in `routes.db` file, but we can use the shortcut `resources(:model)`, e.g. `resources(:directors` or `resources(:movies)` and this will automatically create the seven routes.


### Others:
- `/rails/info/routes`: contains list of all routes


## Some notes on App Dev 2 class:
- Instead of using `appdev-projects/base-rails` repo to create a blank new rails app, we'll now use `appdev-project/vanilla-rails` to create more robust Rails applications from now on.

