# Views and Partials

### How the Pieces Fit Together:
* The controller coordinates the request process and after some messing around with the model, hands the request off to the view, which either returns a whole thing, which is wrapped in a layout, or returns a partial version of it. 

#### The Controller Creates an HTTP response in three ways:
* `render` creates a whole thing (response) to send back to the browser
* `redirect_to` sends an HTTP redirect status code to the browser
* `head` gives you headers from the http request, which (surprise) show up in your browser 

#### Actions inside our controller assume that there is a view responding to the name.

So if we have the following action inside our controller (let's call it our CorgiController):

```ruby
class CorgiController < ApplicationController  
  def be_the_best_dog_ever  
    @ludwig = Corgi.find(params[:id])  
  end  
end
```

then rails will assume that we have a view corresponding to the name of that method:

app/views/corgis/be_the_best_dog_ever.html.erb

##### Sidenote: 
.html.erb acts as two extensions - `html` for the browser and `erb` for rails

### Render vs. Redirect

The main distinction to be made between these two is that `render` just tells rails which view it should use, while `redirect_to` actually creates a whole new thing (request).

render can be placed right in your action. For instance: 

```ruby
class CorgiController < ApplicationController
  def be_the_best_dog_ever
    @ludwig = Corgi.find(params[:id])
    if @ludwig
     render :the_best_messing_dog
    end
  end
end
```
This is terrible code, of course, but whatever. 

### Questions to Anwswer:
####1. How does Rails decide what view to render? Does it depend on what controller I'm inside?

From what I can tell, rails determines which views to render based on the actions in your controller. Generally there is some connection between the two, although a few people are saying that there are ways around this - I still don't entirely get the context for how those are working.

####2. How do I pass variables to my views? To my partials? Is there more than one way?

If you create an instance variable in your action, it should be available in any views and partials you render in that action, by using the erb render tags `<%= @instance_variable %>`

####3. How does Rails differentiate between views and partials?

First off: naming convention. Partials should always be prefixed with and unscore, like so `app/views/pants/_pantalon`

This partial would be rendered in a view like so:
`<%= render partial: "pantalon" %>`

Much more info at: http://api.rubyonrails.org/classes/ActionView/PartialRenderer.html

####4. What are some common ways I can use the render method?

Rendering actions is the most common:
`render :action => "ludwig"`
These are rendered into the layout via `yield` by default, but you can mess around with this like so: 
`render :action => "ludwig", :layout => false` or
`render :action => "ludwig", :layout => "spectacular"` for fun. 

Partials are generally rendered with Ajax calls. By default, the current layout is not used - my guess is that this is because partials are intended to be inserted into an existing page. 

* Partial filenames must be valid Ruby variable names, or they will not work.

Here's an example from API Doc
```Ruby
# Renders the partial, making @new_person available through
# the local variable 'person'
render :partial => "person", :object => @new_person
```

####5. How does one render non-HTML views, e.g., JSON or JavaScript?

You can stick `render json: @ludwig` in your action. I don't necessarily understand the context for this, though. 

The docs say you can render 'vanilla' JavaScript with `render js: "alert('some thing')";`

####6. How does Rails handle layouts? Where do they live?

Layouts live in `app/views/layouts/some_layout.html.erb`
Rails evaluates these before evaluating other parts of the view. It includes other information via:
* Asset tags
* `yield` and `content_for`
* Partials

Asset tag helpers are methods for inserting HTML that can link the view to feeds, JS, stylesheets, images, videos, and audio files.
Read more here: http://pages.citebite.com/g2l2m3d5q7ffe

`yield` is placed in erb tags. It takes whatever view you have select and wraps it in the layout you have specified. It looks like this:
```html
<html>
  <head>
  </head>
  <body>
  <%= yield %>
  </body>
</html>
```
Multiple yields can also be used by specifying where the content is coming from:
```html
<html>
  <head>
    <%= yield :head %>
  </head>
  <body>
  <%= yield %>
  </body>
</html>
```

`content_for` is helpful for inserting distinct regions like sidebars and footers. My impression is that it works more specifically to the view it is contained in, and is better for specific inserts. 

Partials allow you to move part of your code to a separate file. Partials are named with the `_partial_name` convention, and the underscore is removed when rendering the partial:
`<%= render "partial_name" %>`


####7. What should I do if I want a view or partial available to multiple controllers, e.g., a common UI component? Where should that partial live in my application?

I don't know the answer to this question. They probably live in some kind of shared folder, but I'm having trouble finding information on what else needs to happen and I don't fully understand the context of this questions




