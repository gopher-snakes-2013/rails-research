#Routes

##What Are Routes?

"Routes" define certain URL patterns for the application to use. In Sinatra, the routes were always hooked up to "controllers", which would actually run chunks of code when they got URLs they recognized. NO LONGER.

Remember this?
```ruby
get '/boots/:id' do
  @boot = Boot.find(params[:id])
end
```
The "route" in the above code is that red part: `'/boots/:id'`

The "controller" is the block of code that runs when we interact with the route through various methods ('get', 'post', 'put', etc.): `@boot = Boot.find(params[:id])`

***
Everyone knows about those couples who are grossly affectionate and do everything together: hiking, feeding each other cake, and calling each other names like "Sugar" and "Poopsie".

These couples are **obnoxious**, and they run rampant in Sinatraland*, making regular folks gag when they have to go out with them. Rails fixes this problem by splitting them up and telling them to get lives:

ROUTES get banished to 'config/routes.rb' and CONTROLLERS have to live in 'app/controllers'. Destroying romance has never been so satisfying.

###Two Important Things About Controllers in Rails

1. Controllers are just classes. They think they're special, but they're not.
2. All "controller actions" are just instance methods of that class.

###You Haven't Shown Me What a Route Looks Like in Rails

OOPS, here you go:

`get 'clowns/:id => 'circus#view'`

There are three things going on here:

1. We have a route called `clowns/:id`
2. We have a controller called `circus`
3. `circus` has an action/instance method called `view`
  * Instance methods seem to be called with the `#` symbol.

##Advanced Route Tactics

So, those are the basics of routes and, if you want to be super lazy, you can probably just stop here. **HOWEVER**, if you want to be a RoutesMaster, you will need 300 points. You can get 300 points by reading this next part.

###Cheat Codes

Do your fingers get tired of typing out routes? Mine do. Luckily, Rails has been configured for optimal laziness:

```
resources :clowns
```
This is a cheat code that actually generates **seven** different routes for you. They are:

    HTTP Verb |Path             |Controller & Action|Use case
    ----------|-----------------|-------------------|----------------------------
    GET       |/clowns          |=> clown#index     |display a list of all clowns
    GET       |/clowns/new      |=> clown#new       |return an HTML form for creating a new clown
    POST      |/clowns          |=> clown#create    |create a new clown
    GET       |/clowns/:id      |=> clown#show      |display a specific clown
    GET       |/clowns/:id/edit |=> clown#edit      |return an HTML form for editing a clown
    PUT       |/clowns/:id      |=> clown#update    |update a specific clown
    DELETE    |/clowns/:id      |=> clown#destroy   |delete a specific clown

That is a lot of action and, while clowns are humorous and fun to observe, do they really deserve all seven of those routes?

**NOPE**.
###The `:only` Keyword

Maybe I don't really care about showing a unique clown (ALL CLOWNS ARE THE SAME), nor do I care about updating/altering a clown (ONCE A CLOWN, ALWAYS A CLOWN). That means I can ignore a few of those actions above, like **SO**:

`resources :clowns, :only => [:index, :new, :create, :destroy]`

Now, when Rails makes those routes, it will `:only` make those actions in the brackets.

###The `match` Keyword

Perhaps I'm not a fan of the URLs that have been made for me by Rails, and I want to customize things a bit more. Well, I can do that using the `match` keyword instead of a specific HTTP verb, like **SO**:

`match '/silence' => 'clown#destroy'`

Now we can *silence* clowns instead of just deleting them. This isn't RESTful, by any means, but it is funny. Hard to say when this could be useful. Question for Shadi?

###The `:as` Keyword

If you're popular enough, you probably have a nickname, like "Bones" or "Tigger". Routes basically work the same way: if you find yourself referencing a route a lot, and you're getting tired of the full URL, just nickname the address!

For example, I'm probably going to be deleting clowns a lot, so I can save myself time, like **SO**:

`get '/silence' => 'clown#destroy', :as => :silence`

###The `root :to` Command

You can tell your app where to set up shop by defining where it goes for its '/' route, like **SO**:

`root :to => 'questions#index'`

***

*Not a real place.

---
Don't like my sense of humor? Fair enough. Check these other options out:
* Did you like Deaf Grandma? Do it [again][1]!
* Most of this tutorial is blatantly copied from [this guy][2].

[1]: https://gist.github.com/keithtom/3f311c392326bc659b54#readme "Rails the Sinatra Way"
[2]: https://github.com/dontmitch/intro_to_rails/blob/master/Guides/2_routes.md "Intro to Rails: Routes"
