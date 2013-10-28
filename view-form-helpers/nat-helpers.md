form helpers

form_for
fields_for
accepts_nested_attributes_for

view helpers

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

```erb
link_to "Profiles", profiles_path
# => <a href="/profiles">Profiles</a>
```
```erb
link_to "Profile", profile_path(@profile)
# => <a href="/profiles/1">Profile</a>
```

we can produce classes and ids for css:
```erb
link_to "Articles", articles_path, id: "news", class: "article"
# => <a href="/articles" class="article" id="news">Articles</a>
```

[for practice](https://github.com/rguerrettaz/dev_bootcamp_phase3_prep/tree/master/exercises#links)
image_tag