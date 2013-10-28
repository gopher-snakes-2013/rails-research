What's a Controller?
===

`ActionController` is the core library that all controllers we instantiate inherit from in rails. Back in Sinatra land our routes and controllers were tied together in one code block. This made helper methods available in both our views and controllers. In rails, a side effect of abstracting controllers into their own class is that we can add cross-controller behavior by adding methods to `ApplicationController`. These will be available to all of our controllers since each controller we define will be inheriting from the ActionController super class. By default, helper methods defined in the helpers directory are only available to our views. However, we can define helper methods that are accessible to both our controllers and views by using the `#helper_method` method. 

```ruby
class ApplicationController < ActionController::Base
  helper_method :current_user, :logged_in?

  def current_user
    @current_user ||= User.find_by_id(session[:user])
  end

   def logged_in?
     current_user != nil
   end
end
```

http://apidock.com/rails/v3.2.8/AbstractController/Helpers/ClassMethods/helper_method

The above code designates `#current_user` and `#logged_in?` as helper methods that will be available in all controllers and all views. 

What is a before filter?
---

A before filter is a block of code we can define that will be run before each action. This can be something like checking whether a user is logged in, or setting the current user to an instance variable. Because these are steps we don't want to be repeating in every action, extracting them to a before loop helps keep our code DRY.

Por ejemplo:

Bad Smell
```ruby
class PostsController < ApplicationController
  def show
    @post = current_user.posts.find(params[:id])
  end

  def edit
    @post = current_user.posts.find(params[:id])
  end

  def update
    @post = current_user.posts.find(params[:id])
    @post.update_attributes(params[:post])
  end

  def destroy
    @post = current_user.posts.find(params[:id])
    @post.destroy
  end
end
```
Refactor
```ruby
class PostsController < ApplicationController
  before_filter :find_post, :only => [:show, :edit, :update, :destroy]

  def update
    @post.update_attributes(params[:post])
  end

  def destroy
    @post.destroy
  end

  protected    
    def find_post
      @post = current_user.posts.find(params[:id])
    end
end
```
http://rails-bestpractices.com/posts/22-use-before_filter

The `:only` option allows us to specify which actions we want to run the filter before. In the case of user authentication we may not want to bother for sections of your site that are accessible to all users including guests. Alternately, you can change `:only` to `:except` to run your filter before all actions except the ones specified.

Built in Functionality Provided by ApplicationController
---

So what does inheriting from `ApplicationController` get us anyway? 

1. The params hash to which allows us to access information stored as part of the URL (query string parameters) and data passed to our routes through `POST`. 

2. A session hash to store information persistently on a per user basis.

3. A flash hash which persists only long enough to pass information to the next action after a redirect. This is used to display error messages to the user. 

4. Access to cookies stored on users' machines. Like sessions these can be used to persistently store small amounts of data between requests. Unlike a session, this data will persist between sessions (visits) and requests. As long as the user doesn't clear their cookies, you will be able to access this information when they revisit your app. 

5. The ability to render xml and json data.

6. Access to the filters described earlier.

7. A ton of other stuff. http://guides.rubyonrails.org/action_controller_overview.html