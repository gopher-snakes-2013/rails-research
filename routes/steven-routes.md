# Rails - Routes

## Overview
Unlike Sinatra, Rails decouples routes and controllers. In Sinatra, we may have the following:
```Ruby
get '/users' do
  @users = User.all
end
```
This would create a /users route and also create an @users instance variable which was set to all User objects. Rails, on the other hand, separates these actions. Routes are specified in the config/routes.rb file. Using the above example, we may have the following in our config/routes.rb file:
```Ruby
MyApp::Application.routes draw do
  get 'users' => 'user#index'
end
```
We now would have to specify an index action in our controllers/user_controller.rb file like so:
```Ruby
class UserController < ApplicationController
  def index
    @users = User.all
  end
end
```

## FAQ
* Q: How do I know what routes are available in my application?
  * A: To see all routes in your application, you can run ```rake routes``` in your terminal to see a listing of routes. It will be shown in the following format:

```
Route Name(if any) - HTTP Verb(GET, POST, etc) - URL Pattern(/route_name/) -  Params for Route
```

* Q: What does the resources method do in routes.rb? What routes does it add and why?
  * A: The resources method creates 7 different routes in your application for the given controller. For example, using the earlier UserController, we can do the following:

```Ruby
MyApp::Application.routes draw do
  resources :users
end
```

  * And we would get the following routes made for us:

```Ruby
HTTP Verb   Path              Action
GET         /users            index
GET         /users/new        new
POST        /users            create
GET         /users/:id        show
GET         /users/:id/edit   edit
PATCH/PUT   /users/:id        update
DELETE      /users/:id        destroy
```

* Q: What are nested resources?
  * A: Nested resources allow you to capture relationships in your routing. For example, Users have many posts and posts belong to a user. This relationship can be shown using the following nested resource:

```Ruby
MyApp::Application.routes draw do
  resources :users do
    resources :posts
  end
end
```

  * You also need to show this relationship in your models using has_many and belongs_to. Once this relationship is made, you gain access to 7 routes that are created for you using resources, such as

```Ruby
/users/:id/posts
/users/:id/posts/:id
```

* Q: How do I specify my application's root URL?
  * A: In your config/routes.rb file, you need to specify your root route. If you wanted your Users index action to be your route, you would do the following:

```Ruby
MyApp::Application.routes draw do
  root 'users#index'
end
```

## Resources
http://guides.rubyonrails.org/routing.html