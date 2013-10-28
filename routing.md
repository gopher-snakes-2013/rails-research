Compare how Rails and Sinatra approaching routing by giving contrasting code samples
Create diagrams illustrating how Rails' routing system works with the rest of Rails as you understand it
Record a screencast illustrating your points
Curate outside resources

Purpose of the rails router: rails takes incoming requests, asks router to match it to a controller action

##1)Comparison between routing:

###Sinatra
  get '/users/:id' do
    @user = User.find(params[:id])
    erb :index
  end
###Rails
get '/users/:id' => 'pages#index'  #goes in the routes file

  def index
  @user = User.find(params[:id])   #goes in the controller file
  end

  ####pages#index  "pages" is the folder that the index view is contained within. it is important that the controller file that contains our page related methods is named pages_controller. "index" refers to the method call within our controller file.

  if we used 'pages#herpderp', and rails would look for the method 'herpderp' within pages_controller.rb, then the view named herpderp.html.erb. (we could fix this with a redirect)



##2)How do I know what routes are available in my application?
  rake routes displays all paths generated from your routes in this format:
  Prefix   Verb URI Pattern        Controller#Action
            GET  /                  pages#index
  grandma   POST /grandma(.:format) pages#answer

##3)How do I know what URL helpers Rails will generate for my routes?
###button_to(name=nil, options=nil, html_options=nil, &block)
  generates a form containing a single button that submits to the URL created by the set of options. html_options controls form submission and input element behavior. can be
      :method (:post, :get, :patch, :put  -- if none given, defaults to :post)
      :disabled (generates disabled button)
      :remote (by default this behavior is an ajax submit)
      :form (hash of form attributes)
      :form_class (controls class of the form within which the submit button will be placed. by default, class name is defaulted to "button_to" to allow styling of the form and it's children)

  EX:
  <%= button_to "New", action: "new" %>

  # => "<form method="post" action="/controller/new" class="button_to">
  #      <div><input value="New" type="submit" /></div>
  #    </form>"

  <%= button_to "New", { action: "new" }, form_class: "new-thing" %>

  # => "<form method="post" action="/controller/new" class="new-thing">
  #      <div><input value="New" type="submit" /></div>
  #    </form>"

  additional crazier examples halfway down this page: http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html

###current_page?
  true if current request URI was generated by given options
  EXAMPLE: http://www.example.com/shop/checkout?order=desc
    current_page?(action: 'process') #FALSE
    current_page?(action: 'checkout') #TRUE
    current_page?(controller: 'shop', action: 'checkout', order: 'asc') #FALSE

  reworded: true if the options that you pass in match the URI that you're checking.


###link_to(name = nil, options = nil, html_options = nil, &block)
  creates a link tag of the given name created by the string or set of options passed in
    EX: link_to "Profile", @profile
        link_to "Profile", controller: "profiles", action: "show", id: @profile
        #both result in => <a href="/profiles/1">Profile</a>

        you can also add classes & ids to your links for CSS --> id:"idname", class: "classname"

###link_to_if, link_to_unless, link_to_unless_curront, mail_to

##What does the resources method do in routes.rb? What routes does it add and why?
  Example: resources :photos creates 7 different routes in your app. each action maps to particular CRUD functions. 4 URLs match to 7 different actions because of the way the router uses the HTTP Verb and URL to match requests

  HTTP Verb   Path           Action       Used for
  GET        /photos          index    display a list of all photos
  GET        /photos/new       new     return an HTML form for creating a new photo
  POST       /photos          create   create a new photo
  GET        /photos/:id       show    display a specific photo
  GET        /photos/:id/edit  edit    return an HTML form for editing a photo
  PATCH/PUT  /photos/:id      update   update a specific photo
  DELETE     /photos/:id      destroy  delete a specific photo

##How can I configure what routes the resources method generates?


##How do I specify my application's root URL?
at the beginning of your routing file:
  Blog::Application.routes.draw do
    get "welcome/index"
  root: "welcome#index" (or wherever you want to route to)

##What are nested resources? Why do I care about them? Are they worth understanding at the outset?
  rule of thumb: resources should never be nested more than one level deep

  EX:
  resources :magazines do
    resources :ads
  end
  => /magazines/:magazine_id/ads (+ the other 6 routes)

  will also route ads to an AdsController



##If I memorize only one or two things about routing in Rails, what should that be?
rails generate 4 lyfe