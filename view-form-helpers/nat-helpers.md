for now. i'll do better later.

# form helpers

## `form_for`

in sinatra we did:
```html
<form action="/users" method="post">
<input type="text" name="user[username]" placeholder="Username">
<input type="email" name="user[email]" placeholder="email">
<input type="password" name="user[password]" placeholder="Password">
<input type="submit" value="Sign up!">
```
in rails we do:
```erb
<%= form_for @user do |f|  %>
<%= f.text_field :username, placeholder: "Username" %>
<%= f.email_field :email, placeholder: "Email" %>
<%= f.password_field :password, placeholder: "Password" %>
<%= f.submit "Sign up!" %>
<% end %>
```
and that becomes:
```html
<form accept-charset="UTF-8" action="/users" class="new_user" id="new_user" method="post" >
  <div style="margin:0;padding:0;display:inline">
    <input name="utf8" type="hidden" value="✓">
    <input name="authenticity_token" type="hidden" value="fGUew7MnGx+48gel2vjxNhqdHVUKp7q7rIMCIk+ehOw=">
  </div>
  <input id="user_username" name="user[username]" placeholder="Username" size="30" type="text" />
  <input id="user_email" name="user[email]" placeholder="Email" size="30" type="email" />
  <input id="user_password" name="user[password]" placeholder="Password" size="30" type="password" />
  <input name="commit" type="submit" value="Sign up!" />
</form>
```
waala!

the div stuff is just part of rails securrrrity. for now, don't worry bout it.

cred to [ryan](https://github.com/rguerrettaz/dev_bootcamp_phase3_prep/tree/master/overview#forms), [do his tutorial](https://github.com/rguerrettaz/dev_bootcamp_phase3_prep/tree/master/exercises#forms)

## coming soon:
`fields_for`
`accepts_nested_attributes_for`

# view helpers

## `link_to`

in sinatra we did:
```erb
<ul>
  <% if session[:id] %>
    <li><a href="/users/profile">profile</a></li>
    <li><a href="/users/logout">logout</a></li>
  <% else %>
    <li><a href="/users/login">login</a></li>
    <li><a href="/users/signup">signup</a></li>
  <%end %>
</ul>
```
in rails we do:
```erb
<ul>
  <% if current_user %>
    <%= link_to "Profile", "/profile" %>
    <%= link_to "Logout", session_path, :method => :delete %>
  <% else %>
    <%= link_to "Sign up", new_user_path %>
    <%= link_to "Login", new_session_path %>
  <% end %>
</ul>
```
more examples:

```html
link_to(name, url, options = {}, html_options = {})

link_to "Profiles", profiles_path
# => <a href="/profiles">Profiles</a>

link_to "Profile", profile_path(@profile)
# => <a href="/profiles/1">Profile</a>
```

we can produce classes and ids for css:
```html
link_to "Articles", articles_path, id: "news", class: "article"
# => <a href="/articles" class="article" id="news">Articles</a>
```

[for practice](https://github.com/rguerrettaz/dev_bootcamp_phase3_prep/tree/master/exercises#links)
[read the docs](http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to)

## `image_tag`

`image_tag(source, options = {})`

we can add HTML attributes using `options` +
* `:alt` - if none is specified, will default to the source
```html
image_tag("icon.png")  # =>
  <img src="/images/icon.png" alt="Icon" />
```

* `:size => "(WIDTH)x(HEIGHT)"`
```html
image_tag("icon.png", :size => "16x10", :alt => "Edit Entry")  # =>
  <img src="/images/icon.png" width="16" height="10" alt="Edit Entry" />
```
* `:mouseover` - sets an alternate image to be used onmouseover
```html
image_tag("mouse.png", :mouseover => image_path("mouse_over.png")) # =>
  <img src="/images/mouse.png" onmouseover="this.src='/images/mouse_over.png'" onmouseout="this.src='/images/mouse.png'" alt="Mouse" />
```
