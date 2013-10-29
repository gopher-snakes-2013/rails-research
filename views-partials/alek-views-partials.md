#Views and Partials

There are several ridiculous personalities in my extended families, and some of them have strong opinions about things. One of them is named "Rails", and it has very specific **views**. I put jokes in bold so you guys can catch them.

Remember this from Sinatra?
```
get '/sheep/:id' do
  @sheep = Sheep.find(params[:id])

  erb :show_sheep
end
```
In case you don't, here's what's happening:

1. Keep track of a certain user
2. Pass this to an .erb file called 'show_sheep'
3. ...which is really just plain HTML with Ruby code embedded in it.
4. Like jello with fruit chunks. I love that stuff.

Everyone sees the world differently; let's put on some Rails glasses!

```
class SheepController < ApplicationController
  def show
    @sheep = Sheep.find(params[:id])

    # We can actually omit this call to render
    # Rails will render app/views/users/show.html.erb by default
    render :show
  end
end
```
You should have many questions. Here are some of mine:

1. Why is the SheepController inheriting from ApplicationController?
2. What is this `render` nonsense?
3. Who owns all of these sheep and can control them so well?

We need to back up a bit before we can answer these questions (especially that last one).

##Return of the Controller

*Briefly*, this SheepController has a list of methods it knows how to run depending on what routes get passed to it.
