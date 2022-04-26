---
layout: post
title: Class 2 - More helper methods and cleaner codes
---

### More secure way to build forms
- To prevent CSRF (Cross-Site Request Forgery), we'll start adding `authenticity_token` to all forms that use `post` method. For example, this a simple form to create a new movie for this week Helper Methods exercise:
  ```ruby
  <form action="/movies" method="post"> 
    <input
      type="hidden" 
      name="authenticity_token" 
      value="RANDOM_STRING"
  </form>
  ```
  Better yet, we can now use helper method `form_with` in Rails to replace `form` and by default, this method will automatically create the authentication code above instead of us typing all that out every time we create a form. So the code above can be rewritten as:
  ```ruby
  <%= form_with(url: movies_path) do %>
   #Some more code to configure this form here
  <% end %>
  ```
  with `movies_path` is the name for route `/movies` (see **Routes** section below for more details.)
  
- In addition to `form_with` method, there are other helper methods in Rails that can help us construct form easier, more secure, and faster. They are:
  - `<%= label_tag %>` to replace `label` HTML tag. So instead of writing:
    ```ruby
    <label for="title_box"> Title </label>
    ```
    we can re-write that to:
    ```ruby
    <%= label_tag :title_box, "Title" %>
    ```
    
  - `<%= text_field_tag %>` or `<%= number_field_tag %>` or `<%= text_area_tag %>`, etc. to replace different `type` of `input` HTML tag. For example, we'll use `text_field_tag` when `type="text` and `number_field_tag` when `type="number"`. So instead of writing:
    ```ruby
    <input type="text" id="title_box" name="query_title" value="<%= @the_movie.title %>">
    ```
    we can re-write that to:
    ```ruby
    <%= text_field_tag :query_title, @the_movie.title, { id: "title_box"} %>
    ```
    By default, the `text_field_tag` method will use the first argument (in this case, `:query_title`) as both `name` and `id` for this `input` tag. If we want to customize any of these, we can pass a `Hash` argument at the end to do so, in this case, we added `{ id: "title_box"}`.
    
- We'll never write an `<a>` tag again to link two webpages (e.g. to point back to previous page). Instead, we'll use `<%= link_to %>` and `<%= form_with %>`, for example:
  ```ruby
  <%= link_to "Go back", "/movies" %>
  ```
  
- In Rails, for forms that are used to create or update a database records, we can use `form_with(model:)` instead of `form_with(url:)` as long as all our routes are named RESTfully.
  So instead of writing this block of code:
  ```ruby
  <%= form_with(utl: @movie_path) do |form| %>
  <div>
    <%= label_tag :title %>
    <%= text_field_tag "movie[title]", @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>
    <%= text_area_tag "movie[description]", @movie.description, rows: 3 %>
  </div>

  <%= button_tag "Create movie" %>
  <% end %>
  ```
  we can instead write:
  ```ruby
  <%= form_with(model: @movie) do |form| %> 
  <div>
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :description %>
    <%= form.text_area :description, rows: 3 %>
  </div>

  <%= form.button %> 
  <% end %>
  ```
  Notes:
  - We can skip "Create Movie" on the button here because Rails can automatically do that (using the action and the model that we're currently working on.)
  - To make `(model: @movie)` here works, under `create` method in `MoviesController` we'll need to specify that: 
    ```ruby
    movie_attributes = params.require(:movie).permit(:title, :description)
    @movie = Movie.new(movie_attributes)
    ```
    We no longer use `params.fetch()` here, but instead using `params.require()` to specify the Hash that we'll get data from. Additionally, we'll need to tell Rails which database columns can be whitelisted using `permit()`, as Rails will automatically prevent this type of insert from happening (for CSRF reason.)
    
### `.where` vs. `.find_by` vs. `.find` to query a `Hash`:
- In App Dev 1, we often used `Hash.where({}).at(0)` to return the first object because `.where` returns a relation and `.at(0)` returns first record within that relation. Now we can just use `.find_by` to achieve the same thing.
- `.where` will always returns a relation whether it's empty or not, whereas `.find_by` will either return a record or `nil`.
- `.find` can only be used with ID

### Routes:
- In App Dev 1, we were writing code like 
  ```ruby
  get("/", { :controller => "movies", :action => "index"})
  ```
  But now we can use shorthand such as 
  ```ruby
  get "/" => "movies#index"
  ```
  to accomplish the same thing. 
- Better yet, for root/homepage route like the one above, we can use the `root` method and re-write the code to:
  ```ruby
  root "movies#index"
  ```
- We can name a route and refer to it anywhere else in the application. Rails will automatically create these named routes once we configure the routes, as long as there are no dynamic components in the routes. For example, `/` is `root_path`, `/movies` is `movies_path` , but for our dynamic routes to movie detail page `/movies/:id`, we'll need to define what to name it using `as: :details` with `:details` in this case is the name of the route. After naming it, `/movies/:id` is now `details_path`. In other places of the application, we can refer to this path using `<%= details_path(:id) %>`, replacing `:id` with a movie ID. This naming route practice is especially helpful if and when we need to update a route in the future, e.g. from `/movies/:id` to something like `/films/:id`, we will only need to update it once in the `routes.rb` file instead of update the routes everywhere in the codebase, since we're keeping `details_path` as this route's name. 

### Others
- When using an instance variable, such as `@the_movie` on an HTML template (like `show.html.erb`), if we want to refer to a record by its ID, we can either write `@the_movie.id` or just `@the_movie` and Rails will behave the same way.
