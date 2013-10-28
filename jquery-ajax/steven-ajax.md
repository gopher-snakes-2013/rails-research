# Rails - jQuery and AJAX

## Overview

AJAX functionality in Rails (as of Rails 3.0) is easy to start. Instead of having to create the jQuery AJAX call in a Javascript file, you only need to specify ":remote => true" in the link_to or form_for erb method.

By including ":remote => true" in your link_to for form_for method, Rails UJS will add a data-remote="true" HTML5 attribute to the element.

The Rails UJS then looks for all elements with the data-remote attribute set to true and binds common events for that element. Such as click for a link or submit for forms. When that event happens, it creates the $.ajax() call.

So now, instead of having to specify the AJAX call in Javascript like so:
```Javascript
$('a#my_link').on('click', function(event)) {
  $.ajax({
    url: $(this).href,
    type: "GET"
    dataType: "json"
  })
}
```

We only have to do this:
```HTML+ERB
<%= link_to "My Link", my_link_path, :remote => true %>
```

By default, Rails UJS sets the dataType to script. You can change the dataType by including "data-type" => "type" in your link_to or form_for method. For example, if you wanted the above my_link example to specify a dataType of json, you can do the following:
```HTML+ERB
<%= link_to "My Link", my_link_path, :remote => true, "data-type" => "json" %>
```

## FAQ

* Q: How do I create a link that submits a GET request via AJAX in Rails?
  * A: Set :remote => true in the link_to method within your view file. By default, the request type is set to GET. Example:

```HTML+ERB
  <%= link_to "Link Text", link_path, :remote => true %>
```

* Q: How do I make a form submit via AJAX?
  * A: Set :remote => true in the form_for method within your view file. By default, the request type is set to GET. Example:

```HTML+ERB
<%= form_for :model, :remote => true  do |f| %>
  <%= f.label :username %>
  <%= f.text_field :username %>
  <%= f.submit %>
<% end %>
```


* Q: How do I process the response from the server?
  * A: You are still responsible for handling the response from the server. You will want to create a js.erb file based on which action you are working on. For example, if you are working on your index action, you will want to create an index.js.erb file in your views folder (remembering to place it in the correct directory for whichever model you are working on). That .js.erb file will contain the jQuery you want to run.

* Q: How do I control what HTTP verb/method (POST, GET, etc.) my AJAX requests make?
  * A: Sepcify a :method within your ERB. For example, to specify POST in a form submit, do the following:

```HTML+ERB
<%= form_for :model, :method => :post, :remote => true  do |f| %>
  <%= f.label :username %>
  <%= f.text_field :username %>
  <%= f.submit %>
<% end %>
```

## Resources
* http://madebydna.com/all/code/2011/12/05/ajax-in-rails-3.html
* http://railscasts.com/episodes/205-unobtrusive-javascript